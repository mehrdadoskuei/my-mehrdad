# my-mehrdad
just testing monad
# main.py
import sys
import requests
from html import unescape
from bs4 import BeautifulSoup

def get_title(url):
    try:
        resp = requests.get(url, timeout=10)
        resp.raise_for_status()
        soup = BeautifulSoup(resp.text, "html.parser")
        title = soup.title.string.strip() if soup.title and soup.title.string else "No titl found"
        return unescape(title)
    except Exception as e:
        return f"ERROR: {e}"

if __name__ == "__main__":
    if len(sys.argv) < 2:
        print("Usage: python main.py <url>")
        sys.exit(1)
    url = sys.argv[1]
    print(get_title(url))
