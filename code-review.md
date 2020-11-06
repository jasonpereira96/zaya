### Note: Some of the line numbers have changed due to the latest commits

#### **/apis/assistants/index.js** - some HTTP verbs are all small and some are all caps.
#### **/apis/assistants/index.js** (Line 82) - handOver -> handover. The spelling of handover is inconsistent.
#### **/apis/assistants/index.js** (Line 104) - replace the if check with `status = status || "";` More readable.

--- 

#### **/apis/auth/index.js** - some HTTP verbs are all small and some are all caps.
#### **/apis/auth/index.js** - (Line 72) `getCommunicationIDs()` -> `getCommunicationIds()`

---

#### **/apis/common/index.js** - (Line 41) `postCheckList()` -> `postChecklist()` since checklist is one whole word.
#### **/apis/common/index.js** - (Line 72) Here the L in checklist is small. Everywhere else it is capital.
#### **/apis/common/index.js** - (Line 175) Here the L in checklist is small. Everywhere else it is capital. 
#### **/apis/common/index.js** - (Line 200) spelling of `profile_id` is inconsistent with the rest of the file.

---

#### **/apis/customers/index.js** -  Id is spelled differently everywhere! (ID, Id, id)
#### **/apis/customers/index.js** - (Line 10) spelling of `assistantID` is inconsistent with convention.
#### **/apis/customers/index.js** - (Line 46) spelling of `address_id` is inconsistent  with convention.
#### **/apis/customers/index.js** - (Line 62) spelling of `user_id` is inconsistent  with convention.
#### **/apis/customers/index.js** - (Line 200) spelling of `profile_id` is inconsistent with the rest of the file.
#### **/apis/customers/index.js** - (Line 167) magic number should go in constants.
#### **/apis/customers/index.js** - (Line 174) magic number should go in constants.

---

- **components/navbar/index.vue** - Is it just me or is there a LOT of code in this file?
- **components/navbar/index.vue** - (Line 4) The HEADROOM logo could be extracted into its own component (may be overkill)
- **components/navbar/index.vue** - (Line 121) Why is there CSS in the HTML?
- **components/navbar/index.vue** - (Line 227) Can we make `fullName` a computed property instead of using watch?
- **components/navbar/index.vue** - (Line 258) `actionLogoutUser` is a cumbersome name

---

- **components/loader/Loader.vue** - (Line 3) move CSS to the style tag

---

- **components/sidebar/index.vue** - (Line 13) 2 img tags can be condensed into one. There's no need for 2 img tags.
- **components/sidebar/index.vue** - The whole file is littered with duplicate code. Is there a reason for it?
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


- **components/sidebar/index.vue** - (Line 374) Extraneous if-else (in 5 more places)
- **components/sidebar/index.vue** - (Line 421) Extraneous if-else (in 5 more places)
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

- **/config/axios.js** (Line 5) - baseUrl could be in a seperate constants file.

---

- **/config/googleAuth.js** -  a lot of if statements without brackets - Explicit {} for if is better (personal opinion)
- **/config/googleAuth.js** -  move strings to constants file

---

- ~~**/router/index.js** (Line 76) - extract out the auth middleware~~ To remove
```javascript
(to, from, next) => {
    console.log(store.state.authModule.role);
    if (store.state.authModule.role !== "admin") {
        next(false);
    } else {
        next();
    }
} //could be abstracted into a seperate function checkAdmin()
```
and used as such
```javascript
{
    path: "/admin/create-pa-account",
    name: "CreatePAAccount",
    meta: {
      assistants: true,
    },
    component: () =>
        import(
            /* webpackChunkName: "create-pa-account" */ "../views/admin/CreatePAAccount.vue"
        ),
    beforeEnter: checkAdmin
},
```

---

- **/store/index.js** - No Issues

---

- **/store/assistants/index.js** - No Issues

---

- **/store/auth/index.js** - No Issues

---

- **/store/common/index.js** - No Issues

---

- **/utils/constants/index.js** - No Issues

---

- **/utils/applyDrag.js** - No Issues

---

- **/views/admin/customers/CurrentCustomers.vue** - (Line 4) Extract the skeleton loader/s to a seperate file.
- **/views/admin/customers/CurrentCustomers.vue** - How about extracting CurrentCustomerRow into a seperate Component?
- **/views/admin/customers/CurrentCustomers.vue** - Extract the modal into a seperate component. Maybe create a seperate folder of modals -> Create a base modal -> Use *slots* to create child modals as necessary.

---

- **/views/admin/customers/CustomersList.vue** - (Line 152) 
```javascript
getCustomers(this.params)
    .then((res) => {
        // too much logic in this single function. Can you modularize it?
    });
```
- **/views/admin/customers/CustomersList.vue** - (Line 156) Nested filter/map logic should be clearer.

--- 

- **/views/admin/customers/UnassignedCustomers.vue** - (Line 380) redundant `if` in callback function of `filter()` 
- **/views/admin/customers/UnassignedCustomers.vue** - (Line 380) callback function of `filter()`  returns `undefined`
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

- **/views/admin/AddCategory.vue** - Lot of CSS in the HTML. Is this necessary?
- **/views/admin/AddCategory.vue** - Too much HTML in this component. Must be broken down.
- **/views/admin/AddCategory.vue** - Right Sidebar should be in a seperate component. CategoryCard could be a seperate component.
- **/views/admin/AddCategory.vue** - (Line 687) Redundant ternary operator
```javascript
role === 'admin' ? false : true
//should be
role !== 'admin'
```
- **/views/admin/AddCategory.vue** - (Line 708) move color code to a constants file
- **/views/admin/AddCategory.vue** - (Line 708) Could extract this into a seperate utility function `compress()` , since it used more than once. This cumbersome expression is used **6 times** in this file and probably elsewhere as well.
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

- **/views/admin/AddCategory.vue** - (Line 903) Incorrect use of `.map()`. Use `.forEach()` instead. Unless you're doing this to preserve reactivity.

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
- **/views/admin/AddCategory.vue** - (Line 978) Why use `.then()` in an `async` function? Use `await` instead.
- **/views/admin/AddCategory.vue** - (Line 990) Another usecase for the `compress()` function.
- **/views/admin/AddCategory.vue** - (Line 966) Function `uploadAttachments()` has a callback pyramid of doom. :) https://blog.hellojs.org/asynchronous-javascript-from-callback-hell-to-async-and-await-9b9ceb63c8e8

---

### /views/admin/AddCategoryLoader.vue - **AddCategory.vue and this file have 500+ identical lines of code. Why? Can they be merged into a single file?**

---

### /views/admin/AddCustomer.vue - (Line 4) Since almost every screen has breadcrumbs on top, can we make a seperate, flexible, reusable breadcrumb component? 
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

- **/views/admin/AddCustomer.vue** - (Line 203) Each role should have its own constant.
- **/views/admin/AddCustomer.vue** - (Line 229) `.then()` in `async` function. Use `await` instead.

---

- **/views/admin/AssignPAform.vue** - Empty file

---

- **/views/admin/AssignPAtoCustomer.vue** - This component is HUGE.

---

- **/views/admin/CreatePAAccount.vue** -(Line 97) fade is bound to a static value. Is this necessary?
- **/views/admin/CreatePAAccount.vue** -(Line 106) disabled is bound to a static value. Is this necessary?
```html
<b-alert
    v-if="showAlert"
    :fade="true"
    >{{ alertMsg }}</b-alert
>
```

---

- /views/admin/NewCustomerIntake.vue - (Line 305) unused variable `index`

### /views/admin/NewCustomerIntake.vue - (Line 531) function `navigateToCustomers()` has multiple identical definitions in various files

### /views/admin/NewCustomerIntake.vue - (Line 549) Setting array length directly to 0. Why? Replace with a new array instead. 

```javascript
this.totalApiCalls.length = 0; // I have never seen such a statement.
//Should be
this.totalApiCalls = [];
```

### /views/admin/NewCustomerIntake.vue - (Line 565) `totalApiCalls` is a suspicious variable.

Instead of using a reactive variable to check whether all your async calls are complete, use `await Promise.all()` instead.

---

- **/views/assistant/ArchivedCustomers.vue** - (Line 15) HTML maybe redundant

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
- **/views/assistant/ArchivedCustomers.vue** - (Line 39) Can the skeleton loader be another component?
- **/views/assistant/ArchivedCustomers.vue** - (Line 516) The logic for card hover seems convoluted for such a trivial task, unless there's a good reason for it. Can it be simplified? 

### **/views/assistant/ArchivedCustomers.vue** - (Line 516) `showCardHover` and `undoCardHover` have multiple identical definitions in various files

```bash
git grep -A 5 'showCardHover(id) {' ./src/
git grep -A 5 'undoCardHover(id) {' ./src/
```

- **/views/assistant/ArchivedCustomers.vue** - (Line 549) Redundant `if` in `.filter()`.
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

- **/views/assistant/ArchivedCustomers.vue** - (Line 602) Redundant statement in `catch` block.

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

### **/views/assistant/DedicatedCustomers.vue** - (Line 140) Instead of using multiple SVGs for hover and not hover, Could we use a single SVG and change CSS attributes like fill on hover?

### **/views/assistant/DedicatedCustomers.vue** - (Line 532) `getArchivedAccounts()` has multiple identical definitions in various files

```bash
git grep -A 30 'getArchivedAccounts() {' ./src/
```

- **/views/assistant/DedicatedCustomers.vue** - (Line 394) Modal could be a seperate component.

---

/views/assistant/HandoverCustomers.vue - Skipped

---

- **/views/assistant/PADetails.vue** - (Line 747) Semantic use of `.map()`

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


- **/views/assistant/ViewPADirectory.vue** - (Line 330) Use case for `compress()`
- **/views/assistant/ViewPADirectory.vue** - (Line 339) Use a computed proprety for such a huge expression.
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

- **/views/assistant/ViewPADirectory.vue** - (Line 604) `workHoursToInteger()` repeated in 3 different files. That's still ok, not a big deal.

```bash
git grep -A 2 'workHoursToInteger(w' ./src/
```

---

- **/views/auth/ForgotPassword.vue** - (Line 158) redundant variable `valid`

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
### **/views/auth/ForgotPassword.vue** - (Line 98) Can this banner be extracted into a seperate component? The same code is there is 3 files. If I need to change something about the banner, I'll have to modify 3 files.

- **/views/auth/ForgotPassword.vue** - (Line 76) `fade` bound to a static value

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

- **/views/auth/ForgotPassword.vue** - (Line 165) `.then()` in `async` function, Use `await` instead.
- **/views/auth/ForgotPassword.vue** - (Line 200) `.then()` in `async` function, Use `await` instead.

---

- **/views/auth/Login.vue** - (Line 16) What do the URL queries have to do with this `<h2>` element? 
A comment explaining that would be nice.
- **/views/auth/Login.vue** - (Line 267) Many nested `.then()`s. You could benefit from using `async await` here.
- **/views/auth/Login.vue** - (Line 295) Extra { ? 

---

- **/views/auth/ResetPassword.vue** - Skipped

---

- **/views/common/assistant/professional/PAProfessionalGoals.vue** - (Line 97) The `<aside>` could be a seperate component.
- **/views/common/assistant/professional/PAProfessionalGoals.vue** - (Line 181) Use `v-if` and `v-else`. 
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

- **/views/common/assistant/tasks/PATasksCards.vue** - (Line 156) name should be `PATaskCard`, since this component represents only ONE Card. `PATaskCards` is confusing. 

- **/views/common/assistant/tasks/PATasksCards.vue** - (Line 63) Redundant computation

```html
<span
    class="primary--text font-family-regular more--text cursor-pointer"
    v-if="item.categories.length > 4"
    >+
    {{ item.categories.length - item.categories.slice(0, 4).length }}
    more</span
>
```
`item.categories.slice(0, 4).length` will always evaluate to 4, right?