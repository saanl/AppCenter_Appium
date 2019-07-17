## This repo will show the detail of integrating Appium in App Center.


#### Steps
----
* Install appium 1.11.0: `npm install appium@1.11.0 -g` and then start the command: `appium`

![](https://github.com/saanl/AppCenter_Appium/imgs/p1.jpg "pic")

* Check your pom.xml to see if there is a `maven-surefire-plugin` plugin below your `build/pluginManagement/plugins`. If not, please add this plugin:

````
<plugin>
	<artifactId>maven-surefire-plugin</artifactId>
	<version>x.x.x</version>
</plugin>
````

* Copy this [snippet](https://github.com/Microsoft/AppCenter-Test-Appium-Java-Extensions/blob/master/uploadprofilesnippet.xml) into your pom.xml in the <profiles> tag. If there's no <profiles> section in your pom, make one.

* Create the Test code, name as `Test*.java | *Test.java  | *Tests.java  | *TestCase.java`

````
public class TestUI {
    @Rule
    public TestWatcher watcher = Factory.createWatcher();

    private static EnhancedAndroidDriver<MobileElement> driver;

    @Test
    public void test1() throws MalformedURLException {
        File app = new File("D:\\ApiDemos-debug.apk");
        DesiredCapabilities capabilities = new DesiredCapabilities();
        capabilities.setCapability(MobileCapabilityType.PLATFORM_NAME, MobilePlatform.ANDROID);
        capabilities.setCapability(MobileCapabilityType.DEVICE_NAME, "Pixel_2_XL_API_26");
        capabilities.setCapability(MobileCapabilityType.APP, app.getAbsolutePath());
        driver =  Factory.createAndroidDriver(new URL("http://127.0.0.1:4723/wd/hub"),capabilities);
    }

    @After
    public void TearDown(){
        driver.label("Stopping App");
        driver.quit();
    }
}
````

* Run the `mvn surefire:test -f "Your project/pom.xml" ` or Execute the `surefire:test` task

![](https://github.com/saanl/AppCenter_Appium/imgs/p2.jpg "pic")
![](https://github.com/saanl/AppCenter_Appium/imgs/p3.jpg "pic")

* If you see this picture, the maven project works fine.

![](https://github.com/saanl/AppCenter_Appium/imgs/p4.jpg "pic")

* Pack your test classes and all dependencies into the target/upload folder

`mvn -DskipTests -P prepare-for-upload package`

![](https://github.com/saanl/AppCenter_Appium/imgs/p5.jpg "pic")

* Use appcenter command to run the Test

````
appcenter test run appium --app "{OwnerName}/{AppName}" --devices "{OwnerName}/{DeviceSetID}" --app-path "{Your path to apk file}.apk"  --test-series "master" --locale "en_US" --build-dir target/upload
````

![](https://github.com/saanl/AppCenter_Appium/imgs/p6.jpg "pic")
