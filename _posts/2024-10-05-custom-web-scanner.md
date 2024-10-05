---
title: My custom web scanner WebZir
date: 2024-10-05 14:17:00 +0200
categories: [learning, projects]
tags: [cybersecurity]
description: How I developed my own web scanner to assist me in pentest
---

As one of my side projects I really wanted to create my own web scanner. When I was training, I noticed, I had to do same things over and over again. This scanner is a try to automate this routine.

I am not the fan of automated scanners, though. Often they miss important details, so it is better to always check everything manually. However, if you need to check multiple hosts quickly, using scanners might be a good idea.

You can check the full project on my Github: https://github.com/JeniaD/WebZir

The last time I wrote this code was a quite a long time ago (and now when I'm writing this post I am shocked how did I write a code like this, classic indeed), so by the time you are reading it, the project will be hopefuly drastically improved and added new features.

# Idea

So the way this scanner works is pretty simple. It should gather some basic recon and return it back to the user. The question is, what can we get from automated passive/active reconnaisance?
- HTTP headers
- sitemap.xml
- robots.txt
- DNS entries
- Attempt to recognize tech stack
- Directory bruteforce
- Subdomains bruteforce
- Wayback machine

The thing is, scanner doesn't need to automatically detect tech stack. If it will detect `wp-login` page on the website, or `X-Powered-By` HTTP header, the person who works with the tool should see that and will take his next steps accordingly. The less automation, the better (and easier to develop, of course)

# Creation

I decided to do it OOP-way in Python.

There will be 2 files: `webzir.py` and `core.py`. The `webzir.py` will be the entrypoint, the file which user will start. The `core.py` will contain the scanner itself and all the logic necessary to work. I decided to separate it like this so that if anyone would like to use my scanner, it would be just the matter of importing `core.py` into the other project.

Let's take a look at the `webzir.py`.

I wanted to make it look pretty, so I wrote my own `Log()` function, which will replace the standard `print()` to print colorful messages more easily.

```python
def Log(msg, status='?'):
    color = Fore.CYAN
    if status == '+': color = Fore.GREEN
    elif status == '-': color = Fore.RED

    print(f"[{color}{status}{Style.RESET_ALL}] {msg}")

def PrintName(v):
    print(''' __      __      ___.   __________.__        
/  \    /  \ ____\_ |__ \____    /|__|______ 
\   \/\/   // __ \| __ \  /     / |  \_  __ \\
 \        /\  ___/| \_\ \/     /_ |  ||  | \/
  \__/\  /  \___  >___  /_______ \|__||__|   
       \/       \/    \/        \/''' + f" {Fore.GREEN}v{v}{Style.RESET_ALL}\n")
```

Next, here comes the `main()` function. There are a couple of lines to initialize

```python
def main():
    coreModules = Core()
    colorama.init(autoreset=True)
    colorama.ansi.clear_screen()
    PrintName(coreModules.version)

    parser = argparse.ArgumentParser(description=f"Lightweight web scanner for quick recon")
    parser.add_argument("target", help="your target URL")
    parser.add_argument("--output", help="output directory path")
    parser.add_argument("-r", "--random-agent", help="use random user agent", action="store_true")
    parser.add_argument("-v", "--verbose", help="use extensive output", action="store_true")
    parser.add_argument("-f", "--noRedirect", help="don't allow redirect when bruteforcing entries (faster)", action="store_true")
    args = parser.parse_args()
```

Not to say that there is something exciting, `Core()` is the class imported from the `core.py`, `colorama` is a package which helps printing colorful messages in the terminal, and `argparse` is a package to help manage command line arguments.

```python
startScanTime = time.time()
try:
    coreModules.SetTarget(args.target)
    coreModules.Setup(randomUserAgent=args.random_agent, verbose=args.verbose, allowRedirect=not args.noRedirect)

    ...

    coreModules.DetectTech()
    coreModules.ScrapeWordlist()
    coreModules.Wayback()
    coreModules.Whois()
except (RuntimeError, requests.exceptions.ConnectionError) as e:
    Log(f"Fatal error: {e}", status='-')
    Log("Exiting...", status='?')
    exit(1)
except KeyboardInterrupt:
    print()
    Log("Keyboard interruption", status='-')

totalScanTime = round(time.time() - startScanTime, 2)

for finding in coreModules.results:
    # print it out...

if coreModules.wayback: Log(f"Found {len(coreModules.wayback)} link(s) in Wayback machine", status='+')

print()
Log(f"Time elapsed: {totalScanTime}s", status='?')

if args.output:
    if args.verbose: print("[?] Writing data to the files...")
    if not os.path.exists(args.output): os.makedirs(args.output)
    with open(f"{args.output}/report.txt", 'w') as file:
        file.write(f"WebZir scanner v{coreModules.version}\nScan report for the host {coreModules.target.GetFullURL()} ({coreModules.target.IP}) ")
        file.write(f"{datetime.datetime.now()}\n\n")

        for finding in coreModules.results:
            # write it out...

    with open(f"{args.output}/dictionary.txt", 'w') as file:
        for element in coreModules.wordlist: file.write(element + '\n')
    if coreModules.wayback:
        with open(f"{args.output}/wayback.txt", 'w') as file:
            for element in coreModules.wayback: file.write(element + '\n')
```

I shortened the code a little bit as it has little to no importance. The `main()` function just works with input/output.

Let's now proceed to `core.py`

First, I initialize global variables, they will come very handy later.

```python
# Global values
IMPORTANTENTRIES = "common.txt" # Wordlist for something that should be checked to identify server
IMPORTANTHEADERS = "OWASP_dangerousHeaders.txt" # Wordlist for headers that should be checked
USERAGENTS = "userAgents.txt" # List of random user-agents
MAXREQWAIT = 3 # Max wait time between requests
DEFAULTPROTOCOL = "http" # Default protocol
DEFAULTPORT = 80 # Default port
```

So the `DEFAULTPROTOCOL` and `DEFAULTPORT` defines default protocol and port of the target, which user wants to scan, respectively, if user doesn't specify it

The other 3 (`IMPORTANTENTRIES`, `IMPORTANTHEADERS` and `USERAGENTS`) define filenames, from which WebZir will get the wordlists it needs.
- *common.txt* lists important URL entries it needs to check
- *OWASP_dangerousHeaders.txt* is a list I got from OWASP of headers which could give some intel
- *userAgents.txt* is a list of 1000 user agents to use for requests, maybe it would help in bypassing multiple requests restrictions

Next, there are help functions.

```python
# Convert target URL to IP address
def ConvertToIP(url):
    try:
        # Check if the address starts with 'http://' or 'https://'
        match = re.match(r'(https?://)?(.*?)(/)?$', url)
        protocolPrefix, domain, _ = match.groups()

        # Resolve to IPv4 address
        IP = socket.gethostbyname(domain)
        
        return IP
    except (socket.gaierror, TimeoutError):
        return None

# Generate random string
def RandomString():
    res = ""
    chars = "1234567890qwertyuiopasdfghjklzxcvbnm"
    for i in range(random.randint(5, 15)):
        res += random.choice(chars)
    
    return res

# Load wordlist by name
def LoadList(name):
    with open(f"wordlists/{name}", 'r') as file:
        return file.read().split('\n')
```

Then, two classes: `Target` and `Core`.

`Target` was made to easily access target's data like IP, port, full URL, etc.

```python
class Target:
    def __init__(self, hostname=None, ip=None, protocol=DEFAULTPROTOCOL, port=None, path="", timeout=0):
        self.hostname = hostname
        self.IP = ip
        self.protocol = protocol
        self.port = port
        self.path = path
        self.timeout = timeout
    
    def GetFullURL(self):
        if self.port: port = self.port
        else: port = 443 if self.protocol == "https" else 80
        return f"{self.protocol}://{self.hostname}:{port}/{self.path}"
    
    def Parse(self, target):
        if target.startswith("https"): self.protocol = "https"
        elif target.startswith("http"): self.protocol = "http"
        else: self.protocol = DEFAULTPROTOCOL

        target = target.replace("https://", '').replace("http://", '')

        # target = target.replace("//", '/').replace("::", ':')
        buff = target.split('/', 1)[0]
        
        if ":" in buff:
            self.port = int(buff.split(':')[1])
            self.hostname = buff.split(':')[0]
        else:
            self.port = None # DEFAULTPORT
            self.hostname = buff
        
        buff = target.split('/', 1)
        if len(buff) > 1: self.path = buff[1]
        # if self.path and self.path[-1] != '/': self.path += '/'
    
    def Setup(self):
        self.IP = ConvertToIP(self.hostname)
        if not self.IP: raise RuntimeError("Host doesn't seem to be reachable")
        # requests.head(self.GetFullURL())
```

The `Core` class is a bit more complicated.

```python
class Core:
    def __init__(self, target="127.0.0.1") -> None:
        self.version = "0.8"
        self.userAgent = "webzir/" + self.version

        self.target = Target()

        self.results = {}
        self.wordlist = []
        self.wayback = []

        self.debug = False
        self.allowRedirect = False
    
    def SetTarget(self, t):
        self.target.Parse(t)
        self.target.Setup()
    
    def RandomizeUserAgent(self):
        self.userAgent = random.choice(LoadList(USERAGENTS))
    
    def Setup(self, randomUserAgent=False, verbose=False, allowRedirect=True):
        if randomUserAgent: self.RandomizeUserAgent()
        self.debug = verbose
        self.allowRedirect = allowRedirect

    def DetectTech(self):
        ...

    def ScrapeWordlist(self):
        ...

    def Wayback(self):
        ...
    
    def Whois(self):
        ...
```

First 4 methods are helping with setting up proper scan. Last 3 are the ones which do the scan itself.

```python
def DetectTech(self):
    if self.debug: print(f"[v] Getting server headers...")
    
    response = requests.head(self.target.GetFullURL(), headers={"User-Agent": self.userAgent})
    for header in response.headers:
        if header in LoadList(IMPORTANTHEADERS):
            self.results[header] = response.headers[header]
    
    if self.debug: print(f"[v] Server headers received")
    if self.debug: print(f"[v] Checking for availability of bruteforce enumeration...")

    testReqURL = f"{self.target.GetFullURL()}{RandomString()}"
    nonExistentResponse = requests.head(f"{testReqURL}", headers={"User-Agent": self.userAgent}, allow_redirects=True)
    if nonExistentResponse.status_code == 429:
        self.target.timeout = min(MAXREQWAIT, int(nonExistentResponse.headers["Retry-after"])/1000 + 1)
        if self.debug: print(f"[v] Set up Retry-after ({self.target.timeout})")
    elif nonExistentResponse.status_code != 404:
        raise RuntimeError(f"Response for non-existent URL {testReqURL} responded with {nonExistentResponse}")

    if nonExistentResponse.status_code in [429, 404]:
        if self.debug: print(f"[v] Starting bruteforce...")
        
        for variant in LoadList(IMPORTANTENTRIES):
            req = requests.head(f"{self.target.GetFullURL()}{variant}", headers={"User-Agent": self.userAgent}, allow_redirects=self.allowRedirect)
            if req.status_code != 404:
                if not "Interesting findings" in self.results: self.results["Interesting findings"] = []
                self.results["Interesting findings"] += [f"{variant} ({req.status_code})"]
                if self.debug: print(f"[v] Found {variant} ({req.status_code})")
            time.sleep(self.target.timeout)
```

```python
def ScrapeWordlist(self):
    req = requests.get(self.target.GetFullURL(), headers={"User-Agent": self.userAgent}, allow_redirects=True)
    soup = BeautifulSoup(req.content, "html.parser")
    text = soup.find_all(text=True)
    text = ' '.join(text)
    text = text.replace('\n', ' ')
    while "  " in text: text = text.replace("  ", ' ') # Perhaps remove spaces entirely?
    self.wordlist = list(dict.fromkeys([word for word in text.split(' ') if word and len(word) < 10 and len(word) > 1]))
```

```python
def Wayback(self):
    if self.debug: print(f"[v] Searching in Wayback machine...")
    portal = f"http://web.archive.org/cdx/search/cdx?url=*.{self.target.hostname}/*&output=json&fl=original&collapse=urlkey"

    req = requests.get(portal, headers={"User-Agent": self.userAgent})
    result = []
    
    for v in req.json()[1:]:
        if type(v) == list:
            result += v
        else: result += [v]

    result = list(dict.fromkeys(result))

    if result: self.wayback = result
```

```python
def Whois(self):
    if self.target.IP == self.target.hostname: return
    # TODO: add try...except
    req = whois(self.target.hostname)

    res = {}
    if req.registrar: res["Registrar"] = req.registrar
    if req.registrant: res["Registrant"] = req.registrant
    if req.creation_date: res["Creation date"] = req.creation_date[0] if type(req.creation_date) == list else req.creation_date
    if req.updated_date: res["Updated date"] = req.updated_date
    if req.org: res["Organisation"] = req.org
    if req.address: res["Address"] = req.address
    if req.dnssec: res["DNSSEC"] = req.dnssec
    if req.registrant_postal_code: res["Registrant postal code"] = req.registrant_postal_code
    if req.country: res["Country"] = req.country

    if res: self.results["Whois"] = res
```
