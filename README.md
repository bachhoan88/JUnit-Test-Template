# JUnit-Test-Template
<b> Define View ID in XML (Code Sample)</b>
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



