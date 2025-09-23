# m-mehrdad
# Just testing monad (improved version)

import sys 
import requests
from html import unescape
from bs4 import BeautifulSoup

def get_title(url: str) -> str:
    """Fetch page title from a given URL."""
    try:
        resp = requests.get(url, timeout=10, headers={"User-Agent": "Mozilla/5.0"})
        resp.raise_for_status()
        soup = BeautifulSoup(resp.text, "html.parser")
        if soup.title and soup.title.string:
            return unescape(soup.title.string.strip())
        return "⚠️ No title found"
    except requests.exceptions.Timeout:
        return "⏱️ Request timed out"
    except requests.exceptions.RequestException as e:
        return f"❌ Request failed: {e}"
    except Exception as e:
        return f"💥 Unexpected error: {e}"

def main():
    if len(sys.argv) < 2:
        print("Usage: python main.py <url>")
        sys.exit(1)
    url = sys.argv[1].strip()
    print(f"🔗 URL: {url}")
    print(f"📑 Title: {get_title(url)}")

if __name__ == "__main__":
    main()
