# team52
import java.time.Duration;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.testng.annotations.Test;

public class TestFixVersion {
    WebDriver driver;
    WebDriverWait wait;

    @Test
    public void jirahome() {
        System.setProperty("webdriver.chrome.driver", "C:\\Users\\h59277\\Downloads\\chromedriver-win64\\chromedriver.exe");
        driver = new ChromeDriver();
        wait = new WebDriverWait(driver, Duration.ofSeconds(10));
        driver.get("https://jira.cib.echonet/secure/Dashboard.jspa");
        driver.manage().window().maximize();
        System.out.println("--------browser launched--------");

        WebElement searchInput = wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//*[@id='quickSearchInput']")));
        searchInput.click();
        System.out.println("----Search clicked----");

        WebElement userOptions = wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//*[@id='user-options']/a")));
        userOptions.click();
        System.out.println("----Logged in----");

        wait.until(ExpectedConditions.urlContains("Dashboard.jspa"));
        System.out.println("----Dashboard loaded----");

        WebElement viewAllIssues = wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//span[text()='View all issues']")));
        viewAllIssues.click();
        System.out.println("----View all issues clicked----");

        WebElement advancedSearchButton = wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//*[@data-original-title='Switch to advanced search using JQL']")));
        advancedSearchButton.click();
        System.out.println("----Advanced search clicked----");

        WebElement advancedSearchInput = wait.until(ExpectedConditions.visibilityOfElementLocated(By.xpath("//*[@id='advanced-search']")));
        advancedSearchInput.clear();
        System.out.println("----Advanced search input cleared----");
    }
}
