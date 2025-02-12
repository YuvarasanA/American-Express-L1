Framework Structure:
src
├── pages
│    ├── HomePage.java
│    ├── AllCardsPage.java
│    ├── UserDetailsPage.java
├── tests
│    ├── AmericanExpressTest.java
├── base
│    ├── BaseTest.java
├── utils
│    ├── ConfigReader.java

//1️⃣ BaseTest.java (Setup & Teardown)
This class initializes the WebDriver, manages browser configurations, and handles cleanup after execution.
package base;//
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import java.time.Duration;
public class BaseTest {
   protected static WebDriver driver;
   public static void setup() {
       System.setProperty("webdriver.chrome.driver", "path/to/chromedriver");
       driver = new ChromeDriver();
       driver.manage().window().maximize();
       driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
   }
   public static void teardown() {
       if (driver != null) {
           driver.quit();
       }
   }
}
//2️⃣ HomePage.java (Page Object for Homepage)
Handles navigation to the FR Homepage and clicking “Cartes American Express”.//
package pages;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
public class HomePage {
   WebDriver driver;
   // Constructor
   public HomePage(WebDriver driver) {
       this.driver = driver;
   }
   // Locator for "Cartes American Express"
   private By cartesAmericanExpress = By.xpath("//div/p[starts-with(text(),'Cartes American')]");
   // Open homepage
   public void openHomePage() {
       driver.get("https://www.americanexpress.com/fr-fr/?inav=NavLogo");
   }
   // Click on "Cartes American Express"
   public void clickCartesAmericanExpress() {
       WebElement element = driver.findElement(cartesAmericanExpress);
       element.click();
       System.out.println("Clicked 'Cartes American Express'.");
   }
}
//3️⃣ AllCardsPage.java (Page Object for All Cards Page)
Handles clicking “En Savior Plus” under Gold Card.//
package pages;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
public class AllCardsPage {
   WebDriver driver;
   // Constructor
   public AllCardsPage(WebDriver driver) {
       this.driver = driver;
   }
   // Locator for "En Savior Plus" button (Gold Card)
   private By enSavoirPlus = By.xpath("(//div[@class='button     ']/a)[2]");
   // Open the All Cards Page
   public void openAllCardsPage() {
       driver.get("https://www.americanexpress.com/fr/carte-de-paiement/types-cartes/cartes-proprietaires/?intlink=fr-fr-hp-product1-all-pry_cartes-01032021");
   }
   // Click on "En Savior Plus"
   public void clickEnSavoirPlus() {
       WebElement element = driver.findElement(enSavoirPlus);
       element.click();
       System.out.println("Clicked 'En Savior Plus' under Gold Card.");
   }
}
//4️⃣ UserDetailsPage.java (Page Object for User Details Page)
Handles form filling and submitting.//
package pages;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.ui.Select;
public class UserDetailsPage {
   WebDriver driver;
   // Constructor
   public UserDetailsPage(WebDriver driver) {
       this.driver = driver;
   }
   // Locators
   private By firstName = By.xpath("//input[@name='firstName']");
   private By lastName = By.xpath("//input[@name='lastName']");
   private By email = By.xpath("//input[@name='email']");
   private By countryCode = By.xpath("//select[@id='countryCode']");
   private By mobilePhone = By.xpath("//input[@name='mobilePhoneNumber']");
   private By submitButton = By.xpath("//button[@type='submit']");
   // Open User Details Page
   public void openUserDetailsPage() {
       driver.get("https://www.americanexpress.com/fr-fr/charge-cards/apply/personal/gold?sourcecode=A0000FE43V&intlink=fr-amex-cardshop-details-apply-GoldCardAmericanExpress-siderail");
   }
   // Fill form details
   public void fillForm(String fName, String lName, String emailAddress, String mobile) {
       driver.findElement(firstName).sendKeys(fName);
       driver.findElement(lastName).sendKeys(lName);
       driver.findElement(email).sendKeys(emailAddress);
       // Select Country Code
       WebElement countryDropdown = driver.findElement(countryCode);
       Select select = new Select(countryDropdown);
       select.selectByVisibleText("Inde");
       driver.findElement(mobilePhone).sendKeys(mobile);
   }
   // Click on Submit
   public void clickSubmit() {
       driver.findElement(submitButton).click();
       System.out.println("Form submitted successfully.");
   }
}
//5️⃣ AmericanExpressTest.java (Test Class)
Calls all the Page Objects and executes test steps.//
package tests;
import base.BaseTest;
import pages.HomePage;
import pages.AllCardsPage;
import pages.UserDetailsPage;
public class AmericanExpressTest extends BaseTest {
   public static void main(String[] args) {
       // Setup browser
       setup();
       try {
           // Step 1: Open HomePage and Click "Cartes American Express"
           HomePage homePage = new HomePage(driver);
           homePage.openHomePage();
           homePage.clickCartesAmericanExpress();
           // Step 2: Open AllCardsPage and Click "En Savior Plus"
           AllCardsPage allCardsPage = new AllCardsPage(driver);
           allCardsPage.openAllCardsPage();
           allCardsPage.clickEnSavoirPlus();
           // Step 3: Open UserDetailsPage, Fill Form, and Submit
           UserDetailsPage userDetailsPage = new UserDetailsPage(driver);
           userDetailsPage.openUserDetailsPage();
           userDetailsPage.fillForm("John", "Doe", "john.doe@example.com", "9876543210");
           userDetailsPage.clickSubmit();
       } catch (Exception e) {
           e.printStackTrace();
       } finally {
           // Teardown browser
           teardown();
       }
   }
}
