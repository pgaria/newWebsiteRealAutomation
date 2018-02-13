---
title: "Remove Retried Failed Test from TestNG Report!"
date: 2018-02-12
draft: false
description: "Retrying the Failed/Skipped Test again creates the problem of having duplicate entries for the test in TestNG reports, so this article explains the way of removing the duplicate retried Skipped Test."
categories: [ "Automation Testing","Tutorials"]
keywords: "TestNG,Java,retry,ReRun,test,SkippedTest,FailedTest,IRetryAnalyzer,Listener"
image: "/img/testng/dependsOnGroup-method-testng-thumb.png"
logothumb: "/img/logothumb/testng_logo.png"
tags: [
    "Test Automation",
    "TestNG","retry-test","report"
]
---
Sometimes back I wrote an article about running the failed test again using [IRetryAnalyzer](http://javadox.com/org.testng/testng/6.9.4/org/testng/IRetryAnalyzer.html) in TestNG. You can read about [How to Retry failed tests in TestNG ?](https://www.pawangaria.com/post/testng/retry-failed-test-in-testng/).
Retrying the failed Test again creates the problem of having duplicate entries for retried tests in TestNG reports. So when a failed test is being retired and is ends as successful or failed after multiple retry, there are multiple entries in the report with all the retried @Test's considered as Skipped Test and then the final result will looks like the below report.
![retry-count-3-failed-test-report](/img/testng/retry-count-3-failed-test-report.png)

So I decided to write this article to solve the problem and try to remove the retried(Skipped) marked Tests from the final TestNG report file. We will try to make results to be overridden. Such tests should then be only marked as Failed or Successful in Final Report.

### How to Remove the Retried Skipped Tests:
We will create multiple classes here to create the scenario and then removing those Test Failures. We will be mainly changing the default behavior of the TestListenerAdapter class in TestNG.

### MyTestListenerAdapter.class
Create a class that extends the `TestListenerAdapter.Class` from TestNG, which stores all the tests
that were ran and you can retrieve these results using methods like getPassedTests(), getFailedTests() etc. We are going to override the `onFinish(ITestContext context)` method in our class and then check for the Skipped test and remove those from the context before TestNg generates the Report.
```Java
import java.util.Iterator;
import org.testng.ITestContext;
import org.testng.ITestNGMethod;
import org.testng.ITestResult;
import org.testng.TestListenerAdapter;

public class MyTestListenerAdapter extends TestListenerAdapter {

    @Override
    public void onFinish(ITestContext context) {
        Iterator<ITestResult> skippedTestCases = context.getSkippedTests().getAllResults().iterator();
        while (skippedTestCases.hasNext()) {
            ITestResult skippedTestCase = skippedTestCases.next();
            ITestNGMethod method = skippedTestCase.getMethod();
            if (context.getSkippedTests().getResults(method).size() > 0) {
                System.out.println("Removing:" + skippedTestCase.getTestClass().toString());
                skippedTestCases.remove();
            }
        }
    }
}
```

### TestRetryAnalyzer.class
Create a Class that extends the IRetryAnalyzer and have implemented a simple retry logic overriding the following method: `public boolean retry(ITestResult result)`. This class retries tests that failed until they pass or report final failures.
```Java
import org.testng.IRetryAnalyzer;
import org.testng.ITestResult;

public class TestRetryAnalyzer implements IRetryAnalyzer {
    int counter = 1;
    int retryMaxLimit = 3;

    public boolean retry(ITestResult result) {
        if (counter < retryMaxLimit) {
            counter++;
            return true;
        }
        return false;
    }
}
```

### TestRetryTestCases.class
Create a TestNG Class which has `MyTestListenerAdapter.class` as Listener and `TestRetryAnalyzer.class` as method level retry argument. So here you can try to run the Test Class With and Without the `@Listeners(MyTestListenerAdapter.class)` and see the difference in the Report Generation.
```Java
import org.testng.Assert;
import org.testng.annotations.Listeners;
import org.testng.annotations.Test;

@Listeners(MyTestListenerAdapter.class)
public class TestRetryTestCases {

    @Test(retryAnalyzer = TestRetryAnalyzer.class)
    public void firstTestMethod() {
        System.out.println("First test method");
        Assert.assertTrue(true);
    }

    @Test(retryAnalyzer = TestRetryAnalyzer.class)
    public void secondTestMethod() {
        System.out.println("Second test method");
        Assert.assertTrue(false);
    }
}
```
### Output:
Open `index.html` file from the test-output directory of the testNG and verify the results. We can see the difference from the previous report screenshot at the starting of this article.
```output
===============================================
    Default test
    Tests run: 4, Failures: 1, Skips: 2
===============================================

===============================================
Default suite
Total tests run: 2, Failures: 1, Skips: 0
===============================================
```
![skipped-test-method-removed-in-testng-report](/img/testng/skipped-test-method-removed-in-testng-report.png)

#### Note:
If you are running the Test Class in Eclipse or IDE, you will see all the skipped Test in the plugin report as plugins work on Runtime and generate the report. This solution is for the TestNG HTML report only.
