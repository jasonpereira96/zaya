# Code Review

## Things I liked

- CSS. *Bravo!* A really eye-catching design which is viscerally and visually appealing to the user.
- Speed. You'll really built a lot given the amount of time. I wouldn't be able to do that myself.
- Project is well structured. Everything has its place and I know where to find it.
- Responsiveness. Some of the screens are responsive which is a big plus!


## Review comments

### Note: Some of the line numbers have changed due to the latest commits

1] **/apis/assistants/index.js** - some HTTP verbs are all small and some are all caps.

2] **/apis/assistants/index.js** (Line 82) - `handOver` -> `handover`. The spelling of handover is inconsistent.

3] **/apis/assistants/index.js** (Line 104) - replace the if check with `status = status || "";` More readable.

--- 

4] **/apis/auth/index.js** - some HTTP verbs are all small and some are all caps.

5] **/apis/auth/index.js** - (Line 72) `getCommunicationIDs()` -> `getCommunicationIds()`

---

6] **/apis/common/index.js** - (Line 41) `postCheckList()` -> `postChecklist()` since checklist is one whole word.

7] **/apis/common/index.js** - (Line 72) Here the L in checklist is small. Everywhere else it is capital.

8] **/apis/common/index.js** - (Line 175) Here the L in checklist is small. Everywhere else it is capital. 

9] **/apis/common/index.js** - (Line 200) spelling of `profile_id` is inconsistent with the rest of the file.

---

10] **/apis/customers/index.js** -  Id is spelled differently everywhere! (ID, Id, id)

11] **/apis/customers/index.js** - (Line 10) spelling of `assistantID` is inconsistent with convention.

12] **/apis/customers/index.js** - (Line 46) spelling of `address_id` is inconsistent  with convention.

13] **/apis/customers/index.js** - (Line 62) spelling of `user_id` is inconsistent  with convention.

14] **/apis/customers/index.js** - (Line 200) spelling of `profile_id` is inconsistent with the rest of the file.

15] **/apis/customers/index.js** - (Line 167) magic number should go in constants.

16] **/apis/customers/index.js** - (Line 174) magic number should go in constants.

---


17] **components/navbar/index.vue** - Is it just me or is there a LOT of code in this file?

18] **components/navbar/index.vue** - (Line 4) The HEADROOM logo could be extracted into its own component (may be overkill)

19] **components/navbar/index.vue** - (Line 121) Why is there CSS in the HTML?

20] **components/navbar/index.vue** - (Line 227) Can we make `fullName` a computed property instead of using watch?

21] **components/navbar/index.vue** - (Line 258) `actionLogoutUser` is a cumbersome name

---

22] **components/loader/Loader.vue** - (Line 3) move CSS to the style tag

---


23] **components/sidebar/index.vue** - (Line 13) 2 img tags can be condensed into one. There's no need for 2 img tags.

24] **components/sidebar/index.vue** - The whole file is littered with duplicate code. Is there a reason for it?
```html
<img
    v-if="reportsActive"
    class="sidebar-icons"
    src="../../assets/icons/reports_sidebar.svg"
    alt
    />
<img
    v-if="!reportsActive"
    class="sidebar-icons"
    src="../../assets/icons/reports_sidebar_inactive.svg"
    alt
/>
```
Can be:
```html
<img
    class="sidebar-icons"
    :src="`../../assets/icons/reports_sidebar${reportsActive ? '' : '_inactive'}.svg`"
    alt
/>
```
Or something similar.



25] **components/sidebar/index.vue** - (Line 374) Extraneous if-else (in 5 more places)

26] **components/sidebar/index.vue** - (Line 421) Extraneous if-else (in 5 more places)
```javascript
if (this.$route.meta.customers) {
    this.customersActive = true;
} else if (!this.$route.meta.customers) {
    this.customersActive = false;
}
//Could be:
this.customersActive = !!this.$route.meta.customers;
```

---

27] **/config/axios.js** (Line 5) - baseUrl could be in a seperate constants file.

---


28] **/config/googleAuth.js** -  a lot of if statements without brackets - Explicit {} for if is better (personal opinion)

29] **/config/googleAuth.js** -  move strings to constants file

---

30] **/store/index.js** - No Issues

---

31] **/store/assistants/index.js** - No Issues

---

32] **/store/auth/index.js** - No Issues

---

33] **/store/common/index.js** - No Issues

---

34] **/utils/constants/index.js** - No Issues

---

35] **/utils/applyDrag.js** - No Issues

---


36] **/views/admin/customers/CurrentCustomers.vue** - (Line 4) Extract the skeleton loader/s to a seperate file.

37] **/views/admin/customers/CurrentCustomers.vue** - How about extracting CurrentCustomerRow into a seperate Component?

38] **/views/admin/customers/CurrentCustomers.vue** - Extract the modal into a seperate component. Maybe create a seperate folder of modals -> Create a base modal -> Use *slots* to create child modals as necessary.

---

39] **/views/admin/customers/CustomersList.vue** - (Line 152) 
```javascript
getCustomers(this.params)
    .then((res) => {
        // too much logic in this single function. Can you modularize it?
    });
```
40] **/views/admin/customers/CustomersList.vue** - (Line 156) Nested filter/map logic should be clearer.

--- 


41] **/views/admin/customers/UnassignedCustomers.vue** - (Line 380) redundant `if` in callback function of `filter()` 

42] **/views/admin/customers/UnassignedCustomers.vue** - (Line 380) callback function of `filter()`  returns `undefined`
```javascript
.filter((item) => {
    if (
        item.profile === this.activeArchiveHoverId &&
        !item.isDelete
    ) {
    return (
        item.profile === this.activeArchiveHoverId &&
        item.isDelete === false
    );
    }
})

//should be
.filter((item) => item.profile === this.activeArchiveHoverId && item.isDelete)
```
ArchivedCustomers, CurrentCustomers and UnassignedCustomers have a lot of code in common.
Can they be merged into one?
Some functions like `showCardHover()` seem to be identical.

---


43] **/views/admin/AddCategory.vue** - Lot of CSS in the HTML. Is this necessary?

44] **/views/admin/AddCategory.vue** - Too much HTML in this component. Must be broken down.

45] **/views/admin/AddCategory.vue** - Right Sidebar should be in a seperate component. CategoryCard could be a seperate component.

46] **/views/admin/AddCategory.vue** - (Line 687) Redundant ternary operator
```javascript
role === 'admin' ? false : true
//should be
role !== 'admin'
```

47] **/views/admin/AddCategory.vue** - (Line 708) move color code to a constants file

48] **/views/admin/AddCategory.vue** - (Line 708) Could extract this into a seperate utility function `compress()` , since it used more than once. This cumbersome expression is used **6 times** in this file and probably elsewhere as well.
This may not be the exact logic, but the idea is reusibility.

```javascript
attachment.name.length > 20 ? `${attachment.name.substring(0, 17)}...` : `${attachment.name}`

//Should be
compress(attachment.name, 20, 17)
```

```javascript
function compress(string, limit, length) {
    return string.length > limit ? `${string.substring(0, length)}...` : string;
}
```

49] **/views/admin/AddCategory.vue** - (Line 903) Incorrect use of `.map()`. Use `.forEach()` instead. Unless you're doing this to preserve reactivity.

```javascript
this.attachments = this.attachments.map((item) => {
    item.loader = false;
    return item;
});

//Should be
this.attachments.forEach(item => {
    item.loader = false;
});
```

50] **/views/admin/AddCategory.vue** - (Line 978) Why use `.then()` in an `async` function? Use `await` instead.

51] **/views/admin/AddCategory.vue** - (Line 990) Another usecase for the `compress()` function.

52] **/views/admin/AddCategory.vue** - (Line 966) Function `uploadAttachments()` has a callback pyramid of doom. :) https://blog.hellojs.org/asynchronous-javascript-from-callback-hell-to-async-and-await-9b9ceb63c8e8

---

### 53] /views/admin/AddCategoryLoader.vue - **AddCategory.vue and this file have 500+ identical lines of code. Why? Can they be merged into a single file?**

---

### 54] /views/admin/AddCustomer.vue - (Line 4) Since almost every screen has breadcrumbs on top, can we make a seperate, flexible, reusable breadcrumb component? 
It'll probably be a bit tricky though.
```html
<b-breadcrumb>
    <b-breadcrumb-item>
        <h6
        @click="navigateToCustomers"
        class="text-uppercase font-family-medium"
        >
        Customers
        </h6>
    </b-breadcrumb-item>
    <b-breadcrumb-item active>
        <h6 class="text-uppercase font-family-medium">Add Customer</h6>
    </b-breadcrumb-item>
</b-breadcrumb>
```
Could be abstracted into another component, since this is everywhere.


55] **/views/admin/AddCustomer.vue** - (Line 203) Each role should have its own constant.

56] **/views/admin/AddCustomer.vue** - (Line 229) `.then()` in `async` function. Use `await` instead.

---

57] **/views/admin/AssignPAform.vue** - Empty file

---

58] **/views/admin/AssignPAtoCustomer.vue** - This component is HUGE.

---


59] **/views/admin/CreatePAAccount.vue** -(Line 97) fade is bound to a static value. Is this necessary?

60] **/views/admin/CreatePAAccount.vue** -(Line 106) disabled is bound to a static value. Is this necessary?
```html
<b-alert
    v-if="showAlert"
    :fade="true"
    >{{ alertMsg }}</b-alert
>
```

---

61] **/views/admin/NewCustomerIntake.vue** - (Line 305) unused variable `index`

### 62] /views/admin/NewCustomerIntake.vue - (Line 531) function `navigateToCustomers()` has multiple identical definitions in various files

### 63] /views/admin/NewCustomerIntake.vue - (Line 549) Setting array length directly to 0. Why? Replace with a new array instead. 

```javascript
this.totalApiCalls.length = 0; // I have never seen such a statement.
//Should be
this.totalApiCalls = [];
```

### 64] /views/admin/NewCustomerIntake.vue - (Line 565) `totalApiCalls` is a suspicious variable.

Instead of using a reactive variable to check whether all your async calls are complete, use `await Promise.all()` instead.

---

65] **/views/assistant/ArchivedCustomers.vue** - (Line 15) HTML maybe redundant

```html
<img
    v-if="!showListView"
    @click="showListView = !showListView"
    src="../../assets/icons/card_grid.svg"
    alt
    class="cursor-pointer"
/>
<img
    v-if="showListView"
    @click="showListView = !showListView"
    src="../../assets/icons/card_list.svg"
    alt
    class="cursor-pointer"
/>
```
Could be

```html
<img
    @click="toggleListView" 
    :src="showListView ? '../../assets/icons/card_list.svg' : '../../assets/icons/card_grid.svg'"
    alt
    class="cursor-pointer"
/>
```

66] **/views/assistant/ArchivedCustomers.vue** - (Line 39) Can the skeleton loader be another component?

67] **/views/assistant/ArchivedCustomers.vue** - (Line 516) The logic for card hover seems convoluted for such a trivial task, unless there's a good reason for it. Can it be simplified? 

### 68] **/views/assistant/ArchivedCustomers.vue** - (Line 516) `showCardHover` and `undoCardHover` have multiple identical definitions in various files

```bash
git grep -A 5 'showCardHover(id) {' ./src/
git grep -A 5 'undoCardHover(id) {' ./src/
```

69] **/views/assistant/ArchivedCustomers.vue** - (Line 549) Redundant `if` in `.filter()`.
```javascript
.filter((item) => {
    if (
    item.profile === this.activeArchiveHoverId &&
    !item.isDelete
    ) {
    return (
        item.profile === this.activeArchiveHoverId &&
        item.isDelete === false
    );
    }
})

//Should be
.filter(item => item.profile === this.activeArchiveHoverId && !item.isDelete)
//NOTE:
if (condition === true) {
    return true;
} else {
    return false;
}
//is equal to
return condition;
```

70] **/views/assistant/ArchivedCustomers.vue** - (Line 602) Redundant statement in `catch` block.

```javascript
getAssistantCustomers(this.assistantProfileId, params)
    .then((res) => {
        this.customersList = [...res.data.results];
        this.count = res.data.count;
    })
    .catch((err) => {
        console.log(err);
        this.isLoadingCustomers = false; //Redundant statement
    })
    .finally(() => {
        this.isLoadingCustomers = false;
    });
})
```

---

### 71] **/views/assistant/DedicatedCustomers.vue** - (Line 140) Instead of using multiple SVGs for hover and not hover, Could we use a single SVG and change CSS attributes like fill on hover?

### 72] **/views/assistant/DedicatedCustomers.vue** - (Line 532) `getArchivedAccounts()` has multiple identical definitions in various files

```bash
git grep -A 30 'getArchivedAccounts() {' ./src/
```

73] **/views/assistant/DedicatedCustomers.vue** - (Line 394) Modal could be a seperate component.

---

74] **/views/assistant/HandoverCustomers.vue** - Skipped

---

75] **/views/assistant/PADetails.vue** - (Line 747) Semantic use of `.map()`

```javascript
const data = [];
for (let i = 0; i < this.communications.length; i++) {
    data.push({
        id: this.communications[i].id,
        value: this.communications[i].value,
    });
}

//Should be

const data = communications.map(communication => {
    let {id, value} = communication;
    return {
        id, value
    };
});
```

---



76] **/views/assistant/ViewPADirectory.vue** - (Line 330) Use case for `compress()`

77] **/views/assistant/ViewPADirectory.vue** - (Line 339) Use a computed proprety for such a huge expression.
Refer the offical Vue Guide. https://vuejs.org/v2/guide/computed.html#Computed-Properties 

```javascript
{{
    `${
    assistant.assistantProfile &&
    assistant.assistantProfile.workHours
        ? workHoursToInteger(assistant.assistantProfile.workHours)
        : ""
    }`
}}

//should be

{{someComputedProperty}}
```

78] **/views/assistant/ViewPADirectory.vue** - (Line 604) `workHoursToInteger()` repeated in 3 different files. That's still ok, not a big deal.

```bash
git grep -A 2 'workHoursToInteger(w' ./src/
```

---

79] **/views/auth/ForgotPassword.vue** - (Line 158) redundant variable `valid`

```javascript
const valid = true; // Why do this? :)
if (valid) { // redundant
    const data = {
        email: this.email,
    };
    this.emailLoading = true;
    //...
    //...
}
``` 
### 80] **/views/auth/ForgotPassword.vue** - (Line 98) Can this banner be extracted into a seperate component? The same code is there is 3 files. If I need to change something about the banner, I'll have to modify 3 files.

81] **/views/auth/ForgotPassword.vue** - (Line 76) `fade` bound to a static value

```html
<b-alert
    class="alert-btn-top alert-create-pa custom-alert mt-3"
    :show="alertDuration"
    v-if="showAlert"
    :fade="true"
    >{{ alertMsg }}</b-alert
>
```

Should be

```html
<b-alert
    class="alert-btn-top alert-create-pa custom-alert mt-3"
    :show="alertDuration"
    v-if="showAlert"
    fade
    >{{ alertMsg }}</b-alert
>
```
Refer https://bootstrap-vue.org/docs/components/alert#fading-alerts

Or is there a specific reason to bind it?


82] **/views/auth/ForgotPassword.vue** - (Line 165) `.then()` in `async` function, Use `await` instead.

83] **/views/auth/ForgotPassword.vue** - (Line 200) `.then()` in `async` function, Use `await` instead.

---

84] **/views/auth/Login.vue** - (Line 16) What do the URL queries have to do with this `<h2>` element? 
A comment explaining that would be nice.

85] **/views/auth/Login.vue** - (Line 267) Many nested `.then()`s. You could benefit from using `async await` here.

86] **/views/auth/Login.vue** - (Line 295) Extra { ? 

---

87] **/views/auth/ResetPassword.vue** - Skipped

---


88] **/views/common/assistant/professional/PAProfessionalGoals.vue** - (Line 97) The `<aside>` could be a seperate component.

89] **/views/common/assistant/professional/PAProfessionalGoals.vue** - (Line 181) Use `v-if` and `v-else`. 
https://vuejs.org/v2/guide/conditional.html#v-else

```html
<b-button
    v-if="!viewMode"
    class="btn-danger px-5 py-3 btn-pill mt-3 mb-2">
    Add
</b-button>
<b-button
    v-if="viewMode"
    class="btn-danger px-5 py-3 btn-pill mt-3 mb-2">
    Save
</b-button>
```
Should be

```html
<b-button
    v-if="viewMode"
    class="btn-danger px-5 py-3 btn-pill mt-3 mb-2">
    Save
</b-button>
<b-button
    v-else
    class="btn-danger px-5 py-3 btn-pill mt-3 mb-2">
    Add
</b-button>
```
`viewMode` and `!viewMode` is confusing.

---

90] **/views/common/assistant/tasks/PATasksCards.vue** - (Line 156) name should be `PATaskCard`, since this component represents only ONE Card. `PATaskCards` is confusing. 

91] **/views/common/assistant/tasks/PATasksCards.vue** - (Line 63) Redundant computation

```html
<span
    class="primary--text font-family-regular more--text cursor-pointer"
    v-if="item.categories.length > 4"
    >+
    {{ item.categories.length - item.categories.slice(0, 4).length }}
    more</span
>
```
If item.categories.length > 4, then `item.categories.slice(0, 4).length` will always evaluate to 4, right?

### 92] **/views/common/assistant/tasks/PATasksCards.vue** - (Line 159) `userInitials()` has multiple identical definitions.

```bash
git grep -A 3 'userInitials(user)' ./src/
```
Move it to a utils file if possible.

---

93] **/views/common/assistant/tasks/PATasks.vue** - (Line 288) Expression too long to be in template.


94] **/views/common/assistant/tasks/PATasks.vue** - (Line 2) Such a huge skeleton should be in a seperate file!


95] **/views/common/assistant/tasks/PATasks.vue** -Could you render the 5 lists' markup in a for loop? This may be overkill.


96] **/views/common/assistant/tasks/PATasks.vue** - (Line 515) It is not clear why you're using variables like `[one, two, three, four, five]`. Is it necessary to use variable names like these?



97] **/views/common/AddTasks.vue** - (Line 55) Countdown Timer is used in 3 files. Could be moved to its own file.

98] **/views/common/AddTasks.vue** - (Line 980) Why assign `this.categoriesList` twice?

```javascript
//original code
getCategories().then((res) => {
      this.categoriesList = [...res.data.results];
      this.categoriesList = this.categoriesList.map((item) => {
        item.text = item.title;
        return item;
      });
    });

//is there anything wrong with this?
//.map() will anyway create a new array
getCategories().then((res) => {
        this.categoriesList = res.data.results.map((item) => {
        item.text = item.title;
        return item;
    });
});
```

99] **/views/common/AddTasks.vue** - (Line 1282) Redundant ternary operator

100] **/views/common/AddTasks.vue** - (Line 1299) Use a `for .. of` loop here. Avoid using `for(let i=0; ....)` when possible.

101] **/views/common/AddTasks.vue** - (Line 1349) Incorrect use of `.map()`. Value returned by `.map()` is unused. Then why use `.map()` in the first place?
```javascript
const payload = [];
this.selectedCategories.map((item) => {
    payload.push(item.id);
});
//Should be
const payload = this.selectedCategories.map((item) => item.id);
```

102] **/views/common/AddTasks.vue** - (Line 1352) `categoryIDs` should be renamed to `categoryIds` in accordance with convention. 

103] **/views/common/AddTasks.vue** - (Line 1390) Logic for decrementing page is confusing. Logic for incrementing page is good.
```javascript
//original code
prevPage() {
    this.currentPage--;
    if (this.currentPage <= 0) {
        this.currentPage = 1;
    }
}

//should be
prevPage() {
    if (this.currentPage > 1) {
        this.currentPage--;
    }
}
```

---

104] **/views/common/ProfileSettings.vue** - (Line 1461) Bad variable names

```javascript
//bad naming
let valid3 = await this.$refs.addressInfo.validate();
let valid1 = await this.$refs.communicationInfo.validate();
let valid2 = await this.$refs.generalInfo.validate();

//better naming
let addressIsValid = await this.$refs.addressInfo.validate();
let communicationIsValid = await this.$refs.communicationInfo.validate();
let generalIsValid = await this.$refs.generalInfo.validate();
```

105] **/views/common/ProfileSettings.vue** - (Line 1505) Identical definitions in multiple files
```bash
git grep -A 10 'uploadFile' ./src/
```

106] **/views/common/ProfileSettings.vue** - (Line 1543) Network calls effectively synchronous. Use `Promise.all([])` if the calls are not dependent on each other.

```javascript
//Original code
getCommunicationIDs().then((res) => {
    this.communicationArray = [...res.data.results];
    getNotificationIDs().then((res) => { //only after the first call completes will this one start. 
    //Dont need to wait for the first call to complete. Start the second one immediately.
        this.notificationsArray = [...res.data.results];
    });

//better
Promise.all([getCommunicationIDs(), getNotificationIDs()]).then(([commResult, notificationResult]) => {
    this.communicationArray = [...commResult.data.results];
    this.notificationsArray = [...notificationResult.data.results];
});
```

---

107] **/views/common/Resources.vue** - (Line 108) ResourceCard could be a seperate component.

