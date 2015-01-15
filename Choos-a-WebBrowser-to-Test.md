By default the defined browser to execute the tests is FireFox (IceWeasel for Debian users). IceWeasel works perfectly, but on the other hand, the newest FF (>31) give some problems with our installed Selenium version.

For this reason (or maybe because you want to tests some new features with other browsers) you might want to execute the tests with a specific browser instead of FF.

## Define the browser

To chose a different browser you have to define witch one you want throw “Open browser” Keyword. (This Keyword is located in  bika/lims/tests/keywords.txt on develop branch and inside the same test file on hotfix/next):

`Open browser         'url'  browser='browser'`

An example could be: 

`Open browser         http://localhost:55001/plone/login     browser=chrome`
or
`Open browser         ${PLONEURL}/login     browser=chrome`

To know other possible browsers read [the table from Selenium keywords table](http://rtomac.github.io/robotframework-selenium2library/doc/Selenium2Library.html#Open%20Browser)


## Google Chrome and Chromium (In GNU/Linux)

With GoogleChrome we may have some problems, consequently, a exception error will raise: _"WebDriverException: Message: 'ChromeDriver executable needs to be available in the path."_

The WebDriver is a tool designed to support automated testing of web apps.

If GoogleChrome is going to be used to test BikaLims, then we have to follow these simple steps:

1. Download the last ChromeDriver from [this google storage.](http://chromedriver.storage.googleapis.com/index.html) (If you are using Chromium, you won't be able to use the latest version, download another.)
2. Extract the executable and copy it to the expected location of Chrome's executable, for Linux systems is _/usr/bin/google-chrome_
3. Change the executable permissions to allow Selenium to execute chromediver: `chmod 755 chromedriver`

If you have some problems, then you may have to read carefully [this Selenium wiki page.](https://code.google.com/p/selenium/wiki/ChromeDriver)


