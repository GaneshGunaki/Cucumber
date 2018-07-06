src\locators

search.google.btn=xpath://*[@id="tsf"]/div[2]/div[3]/center/input[1]
----------------------------------------------------
constant java

package Utility;

public class Constant {
public static long IMPLICIT_WAIT=5000;
public static long WEBDRIVER_WAIT=5000;


	
}



package Utility;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.net.MalformedURLException;
import java.util.List;
import java.util.Properties;
import java.util.concurrent.TimeUnit;
import org.openqa.selenium.By;
import org.openqa.selenium.NoSuchElementException;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.remote.DesiredCapabilities;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import cucumber.api.java.Before;

import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;

public abstract class TestBase {
	
	public static Properties Repository = new Properties();
	public static File f;
	public static FileInputStream FI;
	public WebElement element = null;
	public static String MobileOSValue=null;
	public static String MobileOS=null;
	public static String browser="Desktop Chrome";
	public static WebDriver driver = null;



	@Before
	public void beforeScenario() throws IOException{
		System.out.println("This will run before the Scenario");
		MobileOSValue = MobileOS;
		loadPropertiesFile();
		DesiredCapabilities capabilities = new DesiredCapabilities();
		try {
			switch (browser) {
			case "Desktop Chrome":
				System.setProperty("webdriver.chrome.driver","C:\\Users\\ganesh.gunaki\\Downloads\\BDD_CucumberZip\\BDD_Cucumber\\src\\Resources\\chromedriver.exe");
				ChromeOptions options = new ChromeOptions();
				options.setExperimentalOption("useAutomationExtension", false);
				driver = new ChromeDriver(options); 
				// driver= new RemoteWebDriver(new URL("http://127.0.0.1:8080/wd/hub"), capabilities);
				break;
			case "Desktop Firefox":
				break;
			}
			capabilities.setCapability("scriptName", "Give Script Name");
		} catch (Exception e) {
			throw e;
		}

	}


	public static void loadPropertiesFile() throws IOException {
		//		f = new File(System.getProperty("user.dir") + "\\src\\main\\java\\locators\\" + MobileOSValue + "\\obj.properties");
		f = new File(System.getProperty("user.dir") + "\\src\\locators\\" + MobileOSValue + "\\obj.properties");
		FI = new FileInputStream(f);
		Repository.load(FI);
		// f = new File(System.getProperty("user.dir")+"\\src\\com\\actiTime\\pageLocators\\loginpage.properties");
	}

	public static WebElement getLocator(String locator) throws Exception {
		String[] split = locator.split(":");
		String locatorType = split[0];
		String locatorValue = split[1];
		if (locatorType.toLowerCase().equals("id"))
			return driver.findElement(By.id(locatorValue));
		else if (locatorType.toLowerCase().equals("name"))
			return driver.findElement(By.name(locatorValue));
		else if ((locatorType.toLowerCase().equals("classname")) || (locatorType.toLowerCase().equals("class")))
			return driver.findElement(By.className(locatorValue));
		else if ((locatorType.toLowerCase().equals("tagname")) || (locatorType.toLowerCase().equals("tag")))
			return driver.findElement(By.className(locatorValue));
		else if ((locatorType.toLowerCase().equals("linktext")) || (locatorType.toLowerCase().equals("link")))
			return driver.findElement(By.linkText(locatorValue));
		else if (locatorType.toLowerCase().equals("partiallinktext"))
			return driver.findElement(By.partialLinkText(locatorValue));
		else if ((locatorType.toLowerCase().equals("cssselector")) || (locatorType.toLowerCase().equals("css")))
			return driver.findElement(By.cssSelector(locatorValue));
		else if (locatorType.toLowerCase().equals("xpath"))
			return driver.findElement(By.xpath(locatorValue));
		else
			throw new Exception("Unknown locator type '" + locatorType + "'");
	}

	public static List<WebElement> getLocators(String locator) throws Exception {
		String[] split = locator.split(":");
		String locatorType = split[0];
		String locatorValue = split[1];
		if (locatorType.toLowerCase().equals("id"))
			return driver.findElements(By.id(locatorValue));
		else if (locatorType.toLowerCase().equals("name"))
			return driver.findElements(By.name(locatorValue));
		else if ((locatorType.toLowerCase().equals("classname")) || (locatorType.toLowerCase().equals("class")))
			return driver.findElements(By.className(locatorValue));
		else if ((locatorType.toLowerCase().equals("tagname")) || (locatorType.toLowerCase().equals("tag")))
			return driver.findElements(By.className(locatorValue));
		else if ((locatorType.toLowerCase().equals("linktext")) || (locatorType.toLowerCase().equals("link")))
			return driver.findElements(By.linkText(locatorValue));
		else if (locatorType.toLowerCase().equals("partiallinktext"))
			return driver.findElements(By.partialLinkText(locatorValue));
		else if ((locatorType.toLowerCase().equals("cssselector")) || (locatorType.toLowerCase().equals("css")))
			return driver.findElements(By.cssSelector(locatorValue));
		else if (locatorType.toLowerCase().equals("xpath"))
			return driver.findElements(By.xpath(locatorValue));
		else
			throw new Exception("Unknown locator type '" + locatorType + "'");
	}

	public WebElement getWebElement(String locator) throws Exception {
		return getLocator(Repository.getProperty(locator));
	}

	public List<WebElement> getWebElements(String locator) throws Exception {
		return getLocators(Repository.getProperty(locator));
	}

	public boolean clickAction(String locator) {
		try {
			element = getWebElement(locator);
		} catch (Exception e1) {
			e1.printStackTrace();
		}
		WebDriverWait wait = new WebDriverWait(driver, Constant.WEBDRIVER_WAIT);
		wait.until(ExpectedConditions.elementToBeClickable(element));
		try {
			element.click();
			Thread.sleep(2000);
			return true;
		} catch (Exception e) {
			e.printStackTrace();
			return false;
		}
	}

	public static void openURL() throws MalformedURLException, AssertionError, NoSuchElementException, Exception {
		System.out.println("\n Launching:  now...");
		try {
			driver.get("www.google.com");
			driver.manage().window().maximize();
			driver.manage().timeouts().implicitlyWait(Constant.IMPLICIT_WAIT, TimeUnit.SECONDS);
			System.out.println("- - Open Home Page URL ");
		} catch (Exception ex) {
			System.out.println("\n Unable to open URL: not sure why" + ex);
		}
	}

	public boolean setText(WebElement element, String testData) {
		WebDriverWait wait = new WebDriverWait(driver, Constant.IMPLICIT_WAIT);
		wait.until(ExpectedConditions.visibilityOf(element));
		try {
			element.sendKeys(testData);
			return true;
		} catch (Exception e) {
			return false;
		}
	}

}














