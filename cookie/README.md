## Cookie Consent Configuration & Web Component Integration

This README provides a complete overview of how to integrate the `mns-cookie` web component into your application with detailed explanation of a sample JSON file used to configure the text, cookies and layout of the cookie policy feature.

---
### Features
1. MNS logo retrieved from CDN
2. Announcement banner
3. Cookie popup policy as per GDPR requirement
---

### MNS Logo
The logo will be retrieved from CDN. The link is: `https://cdn.jsdelivr.net/gh/mnsltd/mns-public@develop/logo.png`

Depending on the project, the logo may be either displayed as an inline image or a background image

1. For inline image:

Replace the `src` atrribute of the `img` tag, making sure that the height attribute is set properly. Example:
```html
<img src="https://cdn.jsdelivr.net/gh/mnsltd/mns-public@develop/logo.png" alt="MNS Logo" height="75" />
```
Please make sure that there is no `width` attribute to avoid distorting the image

2. For background image:

Replace of supercede the appropriate CSS Class/ID with the following CSS property:
```css
background: url(https://cdn.jsdelivr.net/gh/mnsltd/mns-public@develop/logo.png) center right/auto 60px no-repeat;
```
Adjust the `60px` to set the height of the logo. On some projects, the width and height or any other CSS properties of the CSS Class/ID should be fine-tuned for a proper display on the browser.

For instance, for Mastcore projects, add the following code:

```css
.headerBannerLogo {
    background: url(https://cdn.jsdelivr.net/gh/mnsltd/mns-public@develop/logo.png) center right/auto 60px no-repeat;
}
```
---
### Announcement Banner & Cookie Popup policy

Both features are available from a single web component. This web component should be implemented onto the landing page of the project. 

For instance, on Mastcore project, the landing page is `LoginPgStd.xsl`

The web component has 4 main elements:
1. an HTML tag namely `<mns-cookie>`
2. a script that will invoke the web component and render the correct HTML
3. a generic configuration json file shared across all MNS web applications
4. an application-level configuration json file for custom settings - overwriting generic configurations - to be prepared and uploaded onto the CDN prior to implementing the web component

---

## Web Component Implementation

On the landing page of your application, copy and paste the following code:

```html
<!-- Helper Functions -->
<script>
  // Returns the mns-cookie element from the DOM
  const getHost = () => document.querySelector('mns-cookie');

  // Populates the mns-cookie element with provided HTML content
  const populateHost = innerHtml => {
    getHost().innerHTML = innerHtml;
  };

  // Clears the fallback display by emptying the content
  const closeHandler = () => {
    populateHost('');
  };

  // Generates fallback HTML in case the mns-cookie component fails to load
  const getInnerHtml = () => {
    return `<div class="fallback-cookie">
  <div class="fallback-container">
    <div class="close-icon" onclick="closeHandler()">
      <svg width="15" height="15" viewBox="0 0 10 10" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path fill-rule="evenodd" clip-rule="evenodd" d="M10 1L9 0L5 4L1 0L0 1L4 5L0 9L1 10L5 6L9 10L10 9L6 5L10 1Z" fill="black"/>
      </svg>
    </div>
    <p class="fallback-text">This app uses cookies</p>
  </div>
</div>`;
  };
</script>

<!-- Load the mns-cookie Web Component -->
<script
  src="mns-cookie.js"
  type="module"
  onerror="populateHost(getInnerHtml());"
></script>


<mns-cookie
  default-data-url="xxx/mns-cookies-default.json"
  custom-data-url="xxx/mns-cookies-xxx.json"
  application-name="project-xxx"
>
</mns-cookie>

<!-- Event Listener for the mns-cookie component -->
<script>
  // Listen for the 'closed' event, which is emitted when the user dismisses the cookie banner
  getHost().addEventListener('closed', event => {
    const cookieObj = event.detail;
    // Optionally handle specific cookie preferences (e.g., analytics)
    if (!cookieObj.analytics) {
      // To add the Google Analytic code here
    }
  });
</script>
```

Modify the following section with the correct `mns-cookies-default.json`, `mns-cookies-xxx.json` and `project-xxx`:
```
<mns-cookie
  default-data-url="xxx/mns-cookies-default.json"
  custom-data-url="xxx/mns-cookies-xxx.json"
  application-name="project-xxx"
>
</mns-cookie>
```

Details of the mns-cookie web components attributes:

  - **default-data-url** (mandatory): 
      Specifies the generic configuration json file across all MNS web applications. This json file should be retrieved from CDN
  
  - **custom-data-url** (optional): 
      Specifies the application-level configuration json file for custom settings. This json file should be retrieved from CDN
  
  - **application-name** (mandatory): 
      The name used in cookie storage to uniquely identify the application.

---
### Sample application-level JSON file (To be uploaded onto the CDN)
```json

{
  "data": {
    "primaryColor": "#4ED98D" // [Optional] - Used to overwrite the default primary color,
    // announcementBannerConfig: [Customizable at MNS Level and application level] - OPTIONAL
    "announcementBannerConfig": {
      "text": "Hello this is a <span style:'color:red'>MNS Announcement</span>" // Simple text or HTML content,
      "dateFrom": "2025-01-28T07:55:42.049Z"//[Format] - toISOString(),
      "dateTo": "2025-02-04T11:30:33.049Z" //[Format] - toISOString(),
      "textColor": "black" // [Optional],
      "bgColorPrimary": "red" // [Optional],
      "bgColorSecondary": "white" // [Optional]
    },
    // preferences: [Customizable at Application level] - OPTIONAL
    "preferences": [
      {
        "header": {
          // Simple text
          "title": "Marketing",
          // Simple text
          "subtitle": "Delivers personalized ads based on user behavior."
        },
        "cookie": {
          // Simple text
          "name": "marketing"
        },
        // Simple text or HTML content,
        "descriptionText": "Used to track user activity across websites to provide targeted advertising and personalized marketing, ensuring relevant ad experiences."
      },
      {
        "header": {
          "title": "Advertisement",
          "subtitle": "Tracks ad engagement for better targeting."
        },
        "cookie": {
          "name": "advertisement"
        },
        "descriptionText": "Helps deliver relevant ads based on browsing habits and interaction with online content, improving ad relevance and effectiveness."
      }
    ],
    //cookieHeader: [Editable at MNS Level only] - generic and provided by default
    "cookieHeader": {
      "mainHeader": {
        "expiryInDays": 90,
        // Simple text
        "title": "This website uses Cookies.",
        // Simple text or HTML content,
        "subtitle": " We have put some small files called cookies on your device to make our website work and to improve your user experience.  As a result, we may collect and process your site usage data. Let us know what cookies you allow.  We will use a cookie to save your choice. \nBefore making your choice, you can read more about our <a href=\"https://mns.mu/wp-content/uploads/2024/07/DC0-MNS_Cookie-Policy.pdf\">Cookie Policy</a>.\n"
      },
      "consentPreferenceHeader": {
        // Simple text
        "title": "Customize Consent Preferences",
        // Simple text or HTML content,
        "subtitle": "By clicking on the “Save & Close” button, you consent to store on your device all the enabled cookies as described in our <a href=\"https://mns.mu/wp-content/uploads/2024/07/DC0-MNS_Cookie-Policy.pdf\">Cookie Policy</a> for us to access and process your personal data collected. You can read more on how we process your personal data by visiting our Website <a href=\"https://mns.mu/wp-content/uploads/2024/07/DC0-MNS_Privacy-Notice.pdf\">Privacy Notice</a>."
      },
      "necessaryCookie": {
        // Simple text
        "title": "Necessary",
        // Simple text or HTML content,
        "subtitle": "Necessary cookies are required to enable the basic features of this site, such as providing secure log-in or adjusting your consent preferences. These cookies do not store any personally identifiable data."
      }
    },
    // buttonConfig: [Customizable at Application level] - OPTIONAL and provided by default
    "buttonConfig": {
      "headerButtons": [
        {
          "text": "Customize",
          "id": "customize-btn",
          "class": ["customize-btn"],
          "type": "customize"
        },
        {
          "text": "Reject All",
          "id": "reject-btn",
          "class": ["reject-btn"],
          "type": "reject-all"
        },
        {
          "text": "Accept All",
          "id": "accept-btn",
          "class": ["accept-btn"],
          "type": "accept-all"
        }
      ],
      "footerButtons": [
        {
          "text": "Save & Close",
          "type": "save-preference",
          "class": ["cky-btn-preferences"]
        }
      ]
    }
  }
}

```
### JSON Configuration

As per the sample JSON file above, please note:

1. **primaryColor**
  - set it to the colour scheme of your application, preferably the primary colour
2. **announcementBannerConfig**
  - `dateFrom` and `dateFrom` will define start and end date for the announcement banner to appear
3. **preferences**
  - add in the different types of cookies that the application may require. Make sure that `cookie.name` is unique 
4. **cookieHeader**
 - If the cookie expiry should be changed, for whatever reason, please change the following `expiryInDays`