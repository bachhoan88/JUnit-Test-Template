# Basic sample for writing unit tests that mocks the Android framework
*If you are new to unit testing on Android, try this sample first.*

This project uses the Gradle build system and the Android gradle plugin support for unit testing.
You can either benefit from IDEs integration such as Android studio or run the tests on the command
line.

Unit tests run on a local JVM on your development machine. The Android Gradle plugin will compile
your app's source code and execute it using gradle test task. Tests are executed against a modified
version of android.jar where all final modifiers have been stripped off. This lets you use popular
mocking libraries, like Mockito.

For more information see http://tools.android.com/tech-docs/unit-testing-support

## Setup the project in Android studio and run tests.

1. Download the project code, preferably using `git clone`.
1. In Android Studio, select *File* | *Open...* and point to the `./build.gradle` file.
1. Make sure you select "Unit Tests" as the test artifact in the "Build Variants" panel in Android Studio. 
1. Check out the relevant code:
    * The application code is located in `src/main/java`
    * Unit Tests are in `src/test/java`
1. Create a test configuration with the JUnit4 runner: `org.junit.runners.JUnit4`
    * Open *Run* menu | *Edit Configurations*
    * Add a new *JUnit* configuration
    * Choose module *app*
    * Select the class to run by using the *...* button
1. Run the newly created configuration

The unit test will be ran automatically.


## Setup new project in Android Studio
1. Require min SDK 18
1. Config in `build.gradle`
```java
android {
	...
    defaultConfig {
        applicationId "..."
		...
		
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
    ...
}

dependencies {
    androidTestCompile('com.android.support.test.espresso:espresso-core:' + rootProject.espressoVersion, {
        exclude group: 'com.android.support', module: 'support-annotations'
    })
    compile 'com.android.support:appcompat-v7:' + rootProject.appcompatv7Version;
    testCompile 'junit:junit:' + rootProject.testCompileVersion;

    androidTestCompile 'com.android.support.test.uiautomator:uiautomator-v18:' + rootProject.uiautomatorVersion;
    androidTestCompile 'org.hamcrest:hamcrest-integration:' + rootProject.hamcrestVersion;
    ...
}

```

<b> Define View ID in XML (Code Sample)</b>
With a Activity, Developer need write a unit test for that class, and define all ID, function before code
```java
@Test
    public void testConstructor_WIDGET_ID() {
        // Define all widget id using in xml
        onView(withResourceName("textViewHelloWorld")).check(matches(isDisplayed()));
        onView(withResourceName("editText")).check(matches(isDisplayed()));
    }
```
<b> Check default value (sample check hint not null in edit text)</b>
```java
    @Test
    public void testWidgetDefineValid() {
        // check all widget define valid such as: input text has text hint
        onView(withId(R.id.editText)).check(matches(getTextHint()));
    }
    
    public static Matcher<View> getTextHint() {
        return new TypeSafeMatcher<View>() {

            @Override
            public boolean matchesSafely(View view) {
                if (!(view instanceof EditText)) {
                    return false;
                }

                CharSequence hint = ((EditText) view).getHint();
                return !hint.equals("");
            }

            @Override
            public void describeTo(Description description) {
            }
        };
    }
```



