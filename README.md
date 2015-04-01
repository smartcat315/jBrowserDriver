# jBrowserDriver
A programmable embedded browser compatible with the WebDriver Selenium spec -- fast, headless, WebKit-based, 100% pure Java, and no browser dependencies

Licensed under the GNU Affero General Public License version 3 ([details](https://raw.githubusercontent.com/MachinePublishers/jBrowserDriver/master/LICENSE)).

Projects utilizing jBrowserDriver must be licensed as Affero GPLv3 except when on-premise or cloud-based services are [purchased](https://screenslicer.com/pricing) from Machine Publishers, LLC. jBrowserDriver is available bundled with [ScreenSlicer](https://github.com/MachinePublishers/ScreenSlicer), a web scraper.

- - -

#### Download
[Latest release](https://github.com/MachinePublishers/jBrowserDriver/releases/latest)

#### Pre-requisites
There's no need to install any web browser and this works fine on a server (headless). Java 8 (Oracle JDK/JRE or OpenJDK) is required.

Linux users: on Debian/Ubuntu install the following, `apt-get install openjdk-8-jdk openjfx libxslt1.1 libswt-gtk-3-java`

#### Usage
Use this library like any other Selenium WebDriver or RemoteWebDriver (it implements Selenium's JavascriptExecutor, HasInputDevices, TakesScreenshot, Killable, FindsById, FindsByClassName, FindsByLinkText, FindsByName, FindsByCssSelector, FindsByTagName, and FindsByXPath).

You can optionally pass a [Settings](https://github.com/MachinePublishers/jBrowserDriver/blob/master/browser/src/com/machinepublishers/jbrowserdriver/Settings.java#L139) object to the [JBrowserDriver](https://github.com/MachinePublishers/jBrowserDriver/blob/master/browser/src/com/machinepublishers/jbrowserdriver/JBrowserDriver.java#L86) constructor to specify a proxy, request headers, time zone, user agent, or navigator details. By default, the browser mimics the fingerprint of Tor Browser.

Also, you can run as many instances of JBrowserDriver as you want (it's thread safe), and the browser sessions will be fully isolated from each other when run in headless mode, which is the default (when it's run with the GUI shown, some of the memory between instances is shared out of necessity).

Example:

    import org.openqa.selenium.WebDriver;
    import com.machinepublishers.jbrowserdriver.BrowserTimeZone;
    import com.machinepublishers.jbrowserdriver.JBrowserDriver;
    import com.machinepublishers.jbrowserdriver.Settings;
    
    public class Example {
      public static void main(String[] args) {

        // You can optionally pass a Settings object here,
        // constructed using Settings.Builder
        WebDriver driver = new JBrowserDriver(new Settings.Builder().
          browserTimeZone(BrowserTimeZone.AMERICA_NEWYORK).build());

        // This will block for the page load and any
        // associated AJAX requests
        driver.get("http://example.com");

        // You can get status code unlike other Selenium drivers.
        // It blocks for AJAX requests and page loads after clicks 
        // and keyboard events.
        System.out.println(((JBrowserDriver)driver).getStatusCode());

        // Returns the page source in its current state, including
        // any DOM updates that occurred after page load
        System.out.println(driver.getPageSource());
      }
    }
    

#### Global Properties
The following Java system properties can be set:
* `jbd.trace` Log details of every request to standard out. Defaults to `false`.
* `jbd.standarderror` Mirror log output to standard error (the console). Otherwise logs are only available through the Selenium APIs. Defaults to `false`.
* `jbd.browsergui` Show the browser GUI window. Defaults to `false`.
* `jbd.quickrender` Discard web page image data. Recommended because Java is very inefficient (memory-wise) in handling images. Defaults to `true`.
* `jbd.blockads` Whether requests to ad/spam servers should be blocked. Defaults to `true`.
* `jbd.ajaxwait` The idle time required (in milliseconds) before a page is considered to have been loaded completely. This allows the driver to handle ajax. For very slow or overloaded CPUs, set a higher value. Defaults to `600`.
* `jbd.pemfile` Specifies a source of trusted certificate authorities. Otherwise the JRE default keystore is used. Recommended value is: `'https://raw.githubusercontent.com/bagder/ca-bundle/master/ca-bundle.crt'`

Example: `java -Djbd.trace=true -Djbd.ajaxwait=1000 -jar myapp.jar -cp ./jBrowserDriver-v0.4.1.jar my.app.MainClass`

#### Building
No build scripts yet exist, but Eclipse project files are part of this source tree.

#### To-do
Iframes and alert dialogs are not yet handled. Stay tuned.

- - -

Copyright (C) 2014-2015 [Machine Publishers, LLC](https://machinepublishers.com)
