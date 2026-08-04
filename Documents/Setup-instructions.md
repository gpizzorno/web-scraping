# Day 2 Setup

## Python and Jupyter Notebooks

For Day 2 of the workshop, you will need a working installation of [Python](https://www.python.org) and [Jupyter Notebooks](https://jupyterlab.readthedocs.io/en/latest/user/notebook.html). Installation instructions can be found [here](https://github.com/harvard-digital-history/Introduction-to-Python/blob/main/Documents/Getting_started.md). 

>[!TIP]
> If you are not familiar with Python, or need a refresher, you may want to go over Unit 1 of [this workshop](https://github.com/harvard-digital-history/Introduction-to-Python).
>

## Selenium and ChromeDriver

You will also need to install [Selenium](https://www.selenium.dev) and [ChromeDriver](https://chromedriver.chromium.org/home). **Selenium WebDriver** is a multi-platform tool designed to automate web browser interaction. The Python version can be installed through the Anaconda Navigator. Just follow [the instructions to install a package](https://docs.anaconda.com/free/navigator/tutorials/manage-packages/#installing-a-package) and type `selenium` in the **Search Packages** box.

Alternatively, the package can be installed directly from the terminal with the following command:

```bash
pip install selenium
```

**ChromeDriver** is a separate tool that Selenium uses to control Chrome. It is essential that the versions ChromeDriver and Chrome are matched for the system to work. The easiest way to ensure this is by downloading matching versions of both packeges from the [Chrome for Testing availability dashboard](https://googlechromelabs.github.io/chrome-for-testing/#stable). For convenience, here are direct links to the current stable versions for MacOS and Windows:

- **MacOS** (Apple Silicon): [Chrome](https://storage.googleapis.com/chrome-for-testing-public/123.0.6312.122/mac-arm64/chrome-mac-arm64.zip) | [ChromeDriver](https://storage.googleapis.com/chrome-for-testing-public/123.0.6312.122/mac-arm64/chromedriver-mac-arm64.zip)
- **MacOS** (Intel): [Chrome](https://storage.googleapis.com/chrome-for-testing-public/123.0.6312.122/mac-x64/chrome-mac-x64.zip) | [ChromeDriver](https://storage.googleapis.com/chrome-for-testing-public/123.0.6312.122/mac-x64/chromedriver-mac-x64.zip)
- **Windows** (64bit): [Chrome](https://storage.googleapis.com/chrome-for-testing-public/123.0.6312.122/win64/chrome-win64.zip) | [ChromeDriver](https://storage.googleapis.com/chrome-for-testing-public/123.0.6312.122/win64/chromedriver-win64.zip)
- **Windows** (32bit): [Chrome](https://storage.googleapis.com/chrome-for-testing-public/123.0.6312.122/win32/chrome-win32.zip) | [ChromeDriver](https://storage.googleapis.com/chrome-for-testing-public/123.0.6312.122/win32/chromedriver-win32.zip)
