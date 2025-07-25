# Automated-Login-Test-for-a-Web-Application
TEST 3
package tests;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.Assert;
import org.testng.annotations.*;

import pages.LoginPage;

public class Login_Test {
    WebDriver driver;
    LoginPage loginPage;

    @BeforeMethod
    public void setUp() {
        driver = new ChromeDriver();
        driver.get("https://www.saucedemo.com/");
        driver.manage().window().maximize();
        loginPage = new LoginPage(driver);
    }

    @Test
    public void testValidLogin() {
        loginPage.login("standard_user", "secret_sauce");
        Assert.assertTrue(driver.getCurrentUrl().contains("inventory.html"), "Login failed for valid credentials.");
    }

    @Test
    public void testInvalidPassword() {
        loginPage.login("standard_user", "wrong_pass");
        Assert.assertTrue(loginPage.getErrorMessage().contains("Username and password do not match"));
    }

    @Test
    public void testInvalidUsername() {
        loginPage.login("invalid_user", "secret_sauce");
        Assert.assertTrue(loginPage.getErrorMessage().contains("Username and password do not match"));
    }

    @Test
    public void testEmptyFields() {
        loginPage.login("", "");
        Assert.assertTrue(loginPage.getErrorMessage().contains("Username is required"));
    }

    @Test
    public void testEmptyPassword() {
        loginPage.login("standard_user", "");
        Assert.assertTrue(loginPage.getErrorMessage().contains("Password is required"));
    }

    @AfterMethod
    public void tearDown() {
        driver.quit();
    }
}
