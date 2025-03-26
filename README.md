## Cookie Consent Configuration & Web Component Integration

## This README provides a complete overview of how to integrate the `mns-cookie` web component into your application with detailed explanation of a sample JSON file used to configure the text, cookies and layout of the cookie policy feature.

### Features

1. MNS logo retrieved from CDN
2. Announcement banner
3. Cookie popup policy as per GDPR requirement

---

### MNS Logo

The logo will be retrieved from CDN. The link is: `https://cdn.jsdelivr.net/gh/mnsltd/mns-public@develop/logo.png`
Depending on the project, the logo may be either displayed as an inline image or a background image

1. How to replace the inline logo image:
   Replace the `src` atrribute of the `img` tag, making sure that the height attribute is set properly. Example:

```html
<img src="https://cdn.jsdelivr.net/gh/mnsltd/mns-public@develop/logo.png" alt="MNS Logo" height="75" />
```

Please make sure that there is no `width` attribute to avoid distorting the image 2. How to replace background logo image:
Replace of supercede the appropriate CSS Class/ID with the following CSS property:

```css
background: url("https://cdn.jsdelivr.net/gh/mnsltd/mns-public@develop/logo.png") center right/auto 60px no-repeat;
```

Adjust the `60px` to set the height of the logo. On some projects, the width and height or any other CSS properties of the CSS Class/ID should be fine-tuned for a proper display on the browser.
2.1 How to replace background logo image on Mastcore project:
In the CSS file, add the following code at the end of the file:

```css
.headerBannerLogo {
  background: url("https://cdn.jsdelivr.net/gh/mnsltd/mns-public@develop/logo.png") center right/auto 60px no-repeat;
}
```

---

### Announcement Banner & Cookie Popup policy

Both features are available using a single web component, retrieved from the CDN. This web component should be implemented onto the landing page of the project.
For instance, on Mastcore project, the landing page is `LoginPgStd.xsl`
The web component has 4 main elements:

1. an HTML tag namely `<mns-cookie>`
2. a script that will invoke the web component and render the correct HTML
3. a generic configuration json file shared across all MNS web applications
4. an application-level configuration json file for custom settings - overwriting generic configurations - to be prepared and uploaded onto the CDN prior to implementing the web component

---

## Web Component Implementation

### Prepare your application-level configuration json file

Create a new sample json file with the following naming convention: `mns-cookies-xxx.json` replacing the `xxx` with the name of your project.
If there is a need to add a version, please do so.
Copy the code from the sample json file below and paste it onto your newly created json file.\

### Sample application-level JSON file (To be uploaded onto the CDN)

```json
{
  "data": {
    "primaryColor": "#4ED98D", // [Optional] - Used to overwrite the default primary color,
    // announcementBannerConfig: [Customizable at MNS Level and application level] - OPTIONAL
    "announcementBannerConfig": {
      "text": "Hello this is a <span style:'color:red'>MNS Announcement</span>", // Simple text or HTML content,
      "dateFrom": "2025-01-28T07:55:42.049Z", //[Format] - toISOString(),
      "dateTo": "2025-02-04T11:30:33.049Z", //[Format] - toISOString(),
      "textColor": "black", // [Optional],
      "bgColorPrimary": "red", // [Optional],
      "bgColorSecondary": "white" // [Optional]
    },
    // preferences: [Customizable at Project level] - OPTIONAL
    "preferences": [
      {
        "header": {
          "title": "Marketing", //string
          "subtitle": "Delivers personalized ads based on user behavior." //string
        },
        "cookie": {
          "name": "marketing" //string
        },
        "descriptionText": "Used to track user activity across websites to provide targeted advertising and personalized marketing, ensuring relevant ad experiences." // Optional - Simple text or HTML content,

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
    //cookieHeader: [Editable at Application level only] - generic and provided by default
    "cookieHeader": {
      "mainHeader": {
        "expiryInDays": 90,
        // Simple text
        "title": "This website uses Cookies.",
        // Simple text or HTML content,
        "subtitle": " We have put some small files called cookies on your device to make our website work and to improve your user experience. As a result, we may collect and process your site usage data. Let us know what cookies you allow. We will use a cookie to save your choice. \nBefore making your choice, you can read more about our <a href=\"https://mns.mu/wp-content/uploads/2024/07/DC0-MNS_Cookie-Policy.pdf\">Cookie Policy</a>.\n"
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
    // Button Appearance: [Customizable at Project level] - OPTIONAL
    "buttonConfig": {
      "headerButtons": [
        {
          "text": "Customize", // If there is only Essential Cookie for your project, change to "Review"
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

As per the sample JSON file above, please change the following as per the requirement of your project:

- **primaryColor** - set it to the colour scheme of your application, preferably the primary colour
- **announcementBannerConfig.text** - change the text of the announcement
- **announcementBannerConfig** - `dateFrom` and `dateFrom` will define start and end date for the announcement banner to appear
- **preferences** - add in the different types of cookies that the application may require. Make sure that `cookie.name` is unique
- **cookieHeader** - If the cookie expiry should be changed, for whatever reason, please change the following `expiryInDays`. Preferably this section should be reserved to the Application Level json
- **buttonConfig** - To modify the text of the button in the cookie policy popup

#### Important
Once the file is ready, upload it onto the Github repository `mns-public`. Copy the file onto the directory `cookie > json`.

Once uploaded check the file content by clicking on the json link. Review the content and then copy the link from the browser address bar.

Navigate to the following link `https://www.jsdelivr.com/github` and paste the link onto that tool to generate a CDN link. This CDN link will be used in the next step.

---

On the landing page of your application, preferably after the `<body>` tag, copy and paste the following Javascript code:

```html
<!-- Helper Functions -->
<script>
  // Returns the mns-cookie element from the DOM
  const getHost = () => document.querySelector("mns-cookie");
  // Populates the mns-cookie element with provided HTML content
  const populateHost = (innerHtml) => {
    getHost().innerHTML = innerHtml;
  };
  // Clears the fallback display by emptying the content
  const closeHandler = () => {
    populateHost("");
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
<script src="mns-cookie.js" type="module" onerror="populateHost(getInnerHtml());"></script>
<mns-cookie default-data-url="https://cdn.jsdelivr.net/gh/mnsltd/mns-public@develop/cookie/json/mns-cookies-default.json" custom-data-url="xxx/mns-cookies-xxx.json" application-name="project-xxx"> </mns-cookie>
<!-- Event Listener for the mns-cookie component -->
<script>
  // Listen for the 'closed' event, which is emitted when the user dismisses the cookie banner
  getHost().addEventListener("closed", (event) => {
    const cookieObj = event.detail;
    // Optionally handle specific cookie preferences (e.g., analytics)
    if (!cookieObj.analytics) {
      // To add the Google Analytic code here
    }
  });
</script>
```
#### Important
Modify the following section with the correct `mns-cookies-xxx.json` and `project-xxx`:

```
<mns-cookie
default-data-url="https://cdn.jsdelivr.net/gh/mnsltd/mns-public@develop/cookie/json/mns-cookies-default.json"
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
