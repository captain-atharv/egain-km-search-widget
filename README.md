# eGain-KM-Search-Widget

The Search Widget renders an unordered list in an HTML page based on an initialization search string parameter. The list is a result of articles which matches the search parameter from eGain knowledge base. The article results list can also be updated dynamically by calling a javascript function with a corresponding search parameter.


![search-widget](https://user-images.githubusercontent.com/83938216/117610210-f804ec80-b17e-11eb-92d7-ff9aa85213f9.png)


# Prerequisites
1. eGain system
1. eGain knowledge base portal id 
3. Enable CORS in the eGain application. This is required in order to make the web service APIs work without CORS issues. Follow section "Enabling Cross-Origin Resource Sharing" of the product guide "eGain Administrator’s Guide to Administration Console".

# Using The Search Widget
This sample application uses search widget to fetch the corresponding articles from eGain knowledge using the short description. Initially search will be done using pre populated short description. Search results will be updated dynamically as we change the short description.

1. Clone or download this Sample Application.
1. Change below attributes as required in the index html file.
    1. your_kb_portal_id
    1. your_locale
    1. your_template_name
    1. your_egain_b2c_host
    1. your_egain_domain_hint    
1. Now you should be able to fetch the search results from your eGain knowledge portal by loading the index html in browser.


# Configurations
This widget can be configured in the customer's HTML page which can be rendered as desired (dynamically or page load). There is no restriction on combination of widgets i.e., the widgets can be combined with each other in any order in the page.

Each widget follows a pattern in which the attributes of the placeholder HTML element can be configured.

## Widget Library
The library provides a global object "eGainUI" to invoke the functions. It also loads a name-spaced version of JQuery. The name of the JQuery object is same "eGainUI" such that it will not conflict with any other JQuery library customers may have.

The library must be included in the page(s) that are going to enable eGainUI Widgets. The recommendation is to deploy this reference in a global header or footer so the widgets can be deployed dynamically to any page without further site modifications.

Recommendation:
* Please use it in the header: If the JavaScript library is being used by making a javascript function call.
* Please use it in the footer: If the page/document load event is used to trigger the widget.

Script tag to be added to load the eGain JavaScript library is -

```JavaScript
<script type="text/javascript" src="./egain-ui-search-widget.min.js"></script>
```


## Adding Widget to the Page
Once the JavaScript reference to the library is complete, the placeholder HTML elements for widget can simply be added to the page. 

For e.g., the following HTML placeholder DIV will render an unordered list of knowledge articles for the corresponding portal with the search term of "test"


```html
	<div id="eg-search-widget-id"
	    data-egain-role="search-widget"
		data-egain-portal-id="your_kb_portal_id"
		data-egain-template-name="silver"
		data-egain-locale="en-US"
		data-egain-b2c-host="your_egain_b2c_host"
		data-egain-domain-hint="your_egain_domain_hint"
	    >
    </div>
```

The API to call it dynamically is as follows:
```
<script>
    $(document).ready(function () {
        eGainUI('#eg-search-widget-id').SearchWidget("search-term", function () {
            //Code to be executed once the widget is successfully loaded
        });
    });
</script>
```

## Placeholder HTML Element Attributes

|Attribute Name | Description | Type | Value | Default | Required |
|---------------|-------------|------|-------|---------|----------|
|data-egain-b2c-host |eGain B2C host. It is the B2C host for your eGain setup.|	String	| E.g., dev.ezdev.net |	NONE |	Yes |
|data-egain-domain-hint |eGain Domain Hint. It is domain of your eGain setup.|	String	| E.g., abc.egain.cloud |	NONE |	Yes |
|data-egain-template-name|Template of the widget for display. It may be used to control the look and feel of the widget.|	String|	E.g., kiwi |	kiwi |	No |
|data-egain-locale | The locale of the knowledge data.|	String| Format "languagecode-CountryCode". For e.g., "en-US". Should match with the MLKB language. | en-US|	No|
|data-egain-portal-id | Id of the Portal created through eGain KB console (confirm this with eGain deployment team). | Numeric	| E.g., 201700000001000	| NONE	| Yes|


## Styling the output
The library will automatically add eGain-specific CSS classes to each of the elements rendered. It also allows the customer to control the look & feel of the widget through customer specific CSS.

```css
/*
 * This CSS class is added to H1 element rendered
 */
.eGain-Search-Results-h1 {
    /* Add your styles here */
}
  
/*
 * This CSS class is added to each UL element rendered
 */
.eGain-Search-Results-Ul {
    /* Add your styles here */
}
  
/*
 * This CSS class is added to each LI element rendered in the list
 */
.eGain-Search-Results-Li {
    /* Add your styles here */
}
  
/*
 * This CSS Class is added to each anchor tag
 */
.eGain-Search-Results-A {
    /* Add your styles here */
}
```

By providing the definitions to the above CSS classes that is referenced from the page where the widget is being rendered, the non-formatted or non-styled output can be formatted or styled to suit the customer's branding easily. 

## L10N and Callback support:
By default the widget will be loaded with en-US locale and same will be passed to the WS-APIs. In order to fetch the MLKB content update the "data-egain-locale" attribute to fetch the content in desired locale. However the labels and headings will still be in English language. Hence in order to change the labels to the desired language and text callbacks must be used. Callbacks can be registered both for static and dynamically widget.

# Callback in static widget: 
The callback handler must be bound at the end of the page (where static widget is embedded) in a script section. Following is the sample code for registering callback and updating the heading in the callback:

## Callback for static widgets
```
eGainUI('#eg-search-widget-id').on("eGainUI.callback", function () {
    eGainUI('#eg-search-widget-id h1').html("Search Widget - Static");
});
```
## Callback in dynamic widgets: 
The callback handler must be registered when triggering the dynamic widget. Following is the sample code for registering callback and updating the heading in the callback:
```
eGainUI('#eg-search-widget-id').SearchWidget("account", function () {
    eGainUI('#eg-search-widget-id h1').html("Search Widget - Dynamic");
});
```
