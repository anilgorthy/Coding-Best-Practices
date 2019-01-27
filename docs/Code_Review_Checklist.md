## Code Review Checklist

#### Basic guidelines when reviewing code and are specific to Java for Android

##### Code Style
Every project should define code style settings and after that is created/configured in Android Studio and if you use a Mac, you can style/format the files before the merge request is created, like so:

- CTRL + OPT + O to optimize (remove unused) imports
- OPT + CMD + L to style/format the code

##### Coding Guidelines
With the goal of a low cognitive complexity for the code we add, I listed a few things we ought to follow:

- <span style="color:red">Avoid:</span>
	- string concatenation in a loop
	- nested or deep conditions such as `if` within an `if` within an `if` returning null which when unhandled causes `NullPointerException` instead return an empty collection or list and if a method doesn't need a return value, change it to void
	- magic numbers - instead of direct usage of a number, create a string constant which improves readability 
	- passing more than 3-4 parameters to a method instead, create a parameter object
	- empty catch blocks
	- catching generic exceptions instead specify the expected **exception**; for example, instead of catching `Exception`, catch `JSONObjectException`
	- creating public class variables and using a dot operator to access them since it breaks the concept of data encapsulation; instead, create them as private variables and use accessors `getXXX(...)` and mutators `setXXX(...)`

- **Naming**: class, method and variable names should be self-explanatory and leverage (as appropriate) the IDE for generating accessors and mutators, especially for booleans
- **Documentation**: if the naming guideline is followed, documenting the classes and methods is an overkill since class and method names are expected to be self-descriptive and hence only document the complex logic within the code
- **Boy Scout Rule**: leave your code better than you found it
- **DRY**: Never write the same piece of code twice or thrice and if you have to, create a helper/utility method
- <span style="color:red">Add more items here</span>


##### Android specific Coding Guidelines
- **Pre-merge request**: Before putting up a merge request, execute the following command locally to run the unit tests and review/fix reports from static analysis checks:

`$ ./gradlew clean lintRelease testReleaseUnitTest assembleRelease check`


- [Core app quality](https://developer.android.com/docs/quality-guidelines/core-app-quality) and [Multiple screen support](https://developer.android.com/guide/practices/screens_support) provide the basic guidelines when building an Android app
- **Simplified layout**: avoid nested views when possible
- Prefer `Parcelable` (Android interface) over `Serializable` (Java interface)
- Follow a design pattern such as MVP (Model-View-Presenter) pattern which:
	- decreases coupling of the various layers (for example, no data logic in a view)
	- helps unit test the app independent of Android SDK
- Watch for potential window leak errors:
	- `AlertDialog.show()` - always ensure that the activity is visible or not finishing before showing the dialog
	- `ProgressDialog.hide()` or `ProgressBar.hide()` - use dismiss() instead
- Verify if you overrode the appropriate lifecycle methods (`onDestroy()`/`onStop()`/`onPause()`) - to release/unregister resources but also to set dialog/asynctask to null
- <span style="color:red">Add more items here</span>
