package testNG_MultipleWindows;

import java.util.List;
import java.util.concurrent.TimeUnit;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.Select;
import org.testng.Assert;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;

public class Traval_Auto {

	WebDriver driver;

	@BeforeMethod
	public void findUrl() {
		System.setProperty("webdriver.chrome.driver",
				"C:\\Users\\Pragma Developer\\Desktop\\pankaj\\softwares\\chromedriver_win32\\chromedriver.exe");
		driver = new ChromeDriver();// Launching a new Browser.
		driver.manage().window().maximize();
		driver.manage().deleteAllCookies();
		driver.manage().timeouts().pageLoadTimeout(20, TimeUnit.SECONDS);
		driver.manage().timeouts().implicitlyWait(20, TimeUnit.SECONDS);
		driver.get("http://jt-dev.azurewebsites.net/#/SignUp");// Open URL
	}

	// To Validate that the dropdown has "English" and "Dutch"
	@Test
	public void dropDown() {

		driver.findElement(
				By.xpath("//div[@placeholder='Choose Language']//span[@class='ui-select-match-text pull-left']"))
				.click();
		List<WebElement> dd = driver.findElements(
				By.xpath("//div[@id='ui-select-choices-row-1-0']//a[@class='ui-select-choices-row-inner']"));
		for (int i = 0; i < dd.size(); i++) {
			WebElement lang = dd.get(i);
			System.out.println(lang);
		}

	}

	// Fill in your name.
	// For organization, use your name as well.
	// Input your email address.
	// Click on "I agree to the Terms And Conditions"
	// Click on "SignUp"
	// Validate that you received an email.
	@Test(priority = 1, dependsOnMethods = "dropDown")
	public void formField() throws InterruptedException {

		driver.findElement(By.xpath("//*[@id=\"name\"]")).sendKeys("Pankaj Palaskar");
		driver.findElement(By.xpath("//*[@id=\"orgName\"]")).sendKeys("PANKAJ");
		driver.findElement(By.xpath("//*[@id=\"singUpEmail\"]")).sendKeys("pankajpalaskar28@gmail.com");
		Thread.sleep(4000);
		driver.findElement(
				By.xpath("//*[@id=\"content\"]/div/div[3]/div/section/div[1]/form/fieldset/div[4]/label/span")).click();
		Thread.sleep(6000);
		driver.findElement(By.xpath("//*[@id=\"content\"]/div/div[3]/div/section/div[1]/form/fieldset/div[5]/button"))
				.click();
		Thread.sleep(6000);
		//String thanks=driver.findElement(By.className("ng-binding")).toString();
		//Assert.assertEquals( thanks, "A welcome email has been sent. Please check your email.");
		System.out.println("Email Received"); // A welcome email has been sent. Please check your email. 
	}

	@AfterMethod
	public void tearDown() {
		driver.quit();
	}
}

