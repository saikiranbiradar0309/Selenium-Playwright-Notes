# 📝 Input Box in Web Automation

---

## 🔷 What is an Input Box?

An **Input Box** (also called a **Text Field** or **Input Field**) is an interactive HTML element that allows users to enter data — such as text, numbers, passwords, emails, etc.

In HTML, it is represented by the `<input>` tag or `<textarea>` tag.

### Common Types of Input Boxes

| Type | HTML Attribute | Description |
|------|---------------|-------------|
| Text | `type="text"` | Accepts plain text |
| Password | `type="password"` | Masks the entered characters |
| Email | `type="email"` | Accepts email format |
| Number | `type="number"` | Accepts numeric values |
| Search | `type="search"` | Used in search bars |
| Tel | `type="tel"` | Accepts phone numbers |

### Example HTML

```html
<input type="text" placeholder="Enter your email" id="useremail" />
<input type="password" placeholder="Enter your password" id="password" />
```

---

## 🌐 Real-World Example: Naukri.com Login

**Naukri.com** is one of India's largest job portal websites. Its login page contains:
- An **Email/Username** input box
- A **Password** input box
- A **Login** button

> **Login URL:** `https://www.naukri.com/nlogin/login`

### XPaths Used

| Field | XPath |
|-------|-------|
| Email Input | `//input[@placeholder='Enter your active Email ID / Username']` |
| Password Input | `//input[@placeholder='Enter your password']` |
| Login Button | `//button[@type='submit' and text()='Login']` |

---

## ☕ Selenium with Java

### Prerequisites

- Java JDK 8+
- Maven/Gradle project
- Selenium WebDriver dependency
- ChromeDriver or WebDriverManager

### Maven Dependency

```xml
<dependency>
    <groupId>org.seleniumhq.selenium</groupId>
    <artifactId>selenium-java</artifactId>
    <version>4.18.1</version>
</dependency>
<dependency>
    <groupId>io.github.bonigarcia</groupId>
    <artifactId>webdrivermanager</artifactId>
    <version>5.7.0</version>
</dependency>
```

### Selenium Java Code – Naukri.com Login

```java
import io.github.bonigarcia.wdm.WebDriverManager;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

import java.time.Duration;

public class NaukriLoginTest {

    public static void main(String[] args) throws InterruptedException {

        // Setup ChromeDriver using WebDriverManager
        WebDriverManager.chromedriver().setup();
        WebDriver driver = new ChromeDriver();

        // Maximize browser window
        driver.manage().window().maximize();

        // Set implicit wait
        driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));

        // Navigate to Naukri login page
        driver.get("https://www.naukri.com/nlogin/login");

        // XPath for Email Input Box
        WebElement emailInput = driver.findElement(
            By.xpath("//input[@placeholder='Enter your active Email ID / Username']")
        );

        // XPath for Password Input Box
        WebElement passwordInput = driver.findElement(
            By.xpath("//input[@placeholder='Enter your password']")
        );

        // Clear the fields before entering data (best practice)
        emailInput.clear();
        passwordInput.clear();

        // Enter email
        emailInput.sendKeys("your_email@example.com");
        System.out.println("Email entered successfully");

        // Enter password
        passwordInput.sendKeys("your_password@123");
        System.out.println("Password entered successfully");

        // XPath for Login Button
        WebElement loginButton = driver.findElement(
            By.xpath("//button[@type='submit' and text()='Login']")
        );

        // Click Login button
        loginButton.click();
        System.out.println("Login button clicked");

        // Wait and verify login
        Thread.sleep(3000);
        System.out.println("Current URL: " + driver.getCurrentUrl());

        // Close browser
        driver.quit();
    }
}
```

### Key Selenium Methods for Input Box

| Method | Description |
|--------|-------------|
| `sendKeys("value")` | Types text into the input box |
| `clear()` | Clears existing text from the input box |
| `getText()` | Retrieves text (use `getAttribute("value")` for inputs) |
| `getAttribute("value")` | Gets the current value of the input field |
| `isDisplayed()` | Checks if the input box is visible |
| `isEnabled()` | Checks if the input box is interactable |
| `click()` | Focuses on the input box |

---

## 🎭 Playwright with JavaScript

### Prerequisites

- Node.js 14+
- Playwright installed

### Installation

```bash
npm init -y
npm install @playwright/test
npx playwright install
```

### Playwright JS Code – Naukri.com Login (Standalone)

```javascript
const { chromium } = require('@playwright/test');

(async () => {

    // Launch browser (headful mode for visibility)
    const browser = await chromium.launch({ headless: false });

    // Create a new browser context
    const context = await browser.newContext();

    // Open a new page
    const page = await context.newPage();

    // Navigate to Naukri login page
    await page.goto('https://www.naukri.com/nlogin/login');
    console.log('Navigated to Naukri Login Page');

    // XPath for Email Input Box
    const emailInput = page.locator(
        "//input[@placeholder='Enter your active Email ID / Username']"
    );

    // XPath for Password Input Box
    const passwordInput = page.locator(
        "//input[@placeholder='Enter your password']"
    );

    // Wait for the email input to be visible
    await emailInput.waitFor({ state: 'visible' });

    // Clear and type email
    await emailInput.clear();
    await emailInput.fill('your_email@example.com');
    console.log('Email entered successfully');

    // Clear and type password
    await passwordInput.clear();
    await passwordInput.fill('your_password@123');
    console.log('Password entered successfully');

    // XPath for Login Button
    const loginButton = page.locator(
        "//button[@type='submit' and text()='Login']"
    );

    // Click the Login button
    await loginButton.click();
    console.log('Login button clicked');

    // Wait for navigation after login
    await page.waitForTimeout(3000);
    console.log('Current URL:', page.url());

    // Close browser
    await browser.close();

})();
```

### Playwright Test Format (Recommended)

```javascript
const { test, expect } = require('@playwright/test');

test('Naukri Login Test using XPath', async ({ page }) => {

    // Navigate to Naukri login page
    await page.goto('https://www.naukri.com/nlogin/login');

    // XPath for Email Input Box
    const emailInput = page.locator(
        "//input[@placeholder='Enter your active Email ID / Username']"
    );

    // XPath for Password Input Box
    const passwordInput = page.locator(
        "//input[@placeholder='Enter your password']"
    );

    // Wait for email input and fill
    await emailInput.waitFor({ state: 'visible' });
    await emailInput.fill('your_email@example.com');

    // Fill password
    await passwordInput.fill('your_password@123');

    // XPath for Login Button
    const loginButton = page.locator(
        "//button[@type='submit' and text()='Login']"
    );

    // Click login
    await loginButton.click();

    // Wait for page to load
    await page.waitForTimeout(3000);

    // Assert URL has changed (successful login redirects away from login page)
    expect(page.url()).not.toContain('/nlogin/login');
    console.log('Login successful!');
});
```

### Key Playwright Methods for Input Box

| Method | Description |
|--------|-------------|
| `fill('value')` | Clears field and types text (recommended) |
| `type('value')` | Types text character by character |
| `clear()` | Clears existing text from the input box |
| `inputValue()` | Gets the current value of the input field |
| `waitFor({ state: 'visible' })` | Waits until the element is visible |
| `isVisible()` | Checks if the input box is visible |
| `isEnabled()` | Checks if the input box is enabled |
| `press('Enter')` | Simulates pressing a keyboard key |

---

## Selenium vs Playwright – Input Box Comparison

| Feature | Selenium (Java) | Playwright (JS) |
|---------|----------------|-----------------|
| Type Text | `sendKeys("text")` | `fill("text")` |
| Clear Field | `clear()` | `clear()` |
| Get Value | `getAttribute("value")` | `inputValue()` |
| Wait for Element | `WebDriverWait` | `waitFor()` built-in |
| XPath Support | `By.xpath("...")` | `page.locator("//...")` |
| Auto-wait | Manual waits needed | Auto-waits by default |

---

## Tips and Best Practices

1. **Always `clear()` before `sendKeys()`** – Input boxes may have pre-filled values.
2. **Use `getAttribute("value")`** in Selenium (not `getText()`) to get the typed value.
3. **Use `fill()` over `type()`** in Playwright – `fill()` is faster and more reliable.
4. **Use explicit waits** instead of `Thread.sleep()` in Selenium for better stability.
5. **XPath with `placeholder`** is a reliable locator strategy when `id` is not available.
6. **Playwright auto-waits** – No need to add explicit waits for most interactions.

---

*Notes by: Sai Kiran Biradar*  
*Topic: Input Box – Web Automation*
