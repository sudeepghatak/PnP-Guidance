
# SharePoint site branding and page customization solutions
Use the SharePoint page model and composed looks, the SharePoint 2013 theming engine, and CSS to brand your SharePoint site and pages.

 **Last modified:** April 09, 2015

 _**Applies to:** Office 365 | SharePoint 2013 | SharePoint Online_

 **In this article**

 [Key terms and concepts related to SharePoint branding](#sectionSection0)

 [In this section](#sectionSection1)

 [Additional resources](#bk_addresources)


You can customize the look and feel of a SharePoint site in two ways:

- By using the theming engine to create custom themes (composed looks in SharePoint 2013 and SharePoint Online). At a minimum, themes define colors. A complete theme defines colors, fonts, a background image, and the associated master page, and a .preview file that defines how the .master page is previewed. You can use the remote provisioning pattern to apply themes to sites.
    
- By creating custom cascading style sheets (CSS) to apply to SharePoint Online sites. You can use an app for SharePoint and the remote provisioning pattern to provision SharePoint sites to use custom CSS. 
    
Branding changes range from low-cost and simple to high-cost and complex. Users can use the UI to apply composed looks, which include a background image, color palette, fonts, a master page, and an associated .preview file for the master page. You can use the SharePoint 2013 theming engine to design composed looks and provision sites, and you can create custom CSS to modify the look and feel of your site and its elements.

**Important**  Although it's possible to create custom master pages and other structural elements as part of a custom branding project, the long-term cost of supporting custom master pages and other custom structural elements is high. Custom branding can make it more costly for your organization to apply upgrades and provide ongoing support.

This section builds on your knowledge of the and  [design tools and design packaging options](6ec1cef5-05ff-4b6a-9a7b-d02c5e9b30b8.md) to show you how to customize the look and feel of a SharePoint site.

## Key terms and concepts related to SharePoint branding
<a name="sectionSection0"> </a>



|**Term or concept**|**Definition**|**More information**|
|:-----|:-----|:-----|
|Alternate CSS|A CSS file other than the default that you can apply to the look and feel of your site.|Use alternate CSS to apply custom CSS to a site and all of its subsites. |
|CSS|A language that tells a browser how to render an HTML or XML document's styles. CSS separates document content (HTML or XML) from how the content is presented.||
|Composed look|A combination of fonts, a color palette, a background image, and an associated master page that are applied to the site. Font scheme and color images are optional. The following is the default file location for composed looks: Theme Gallery\15 folder|Composed looks are a convenient way to change the look and feel of sites without making any changes to the structure of a site.<br/>SharePoint 2013 ships several composed looks by default. When a user applies a composed look, SharePoint applies all the associated design elements of the composed look to a site.|
|Content Search Web Part (CSWP)|Renders content from search results based on a specified query.| [Content Search Web Part in SharePoint 2013](http://social.technet.microsoft.com/wiki/contents/articles/15843.content-search-web-part-in-sharepoint-2013.aspx) (TechNet) <br/>[Content Search Web Part in SharePoint 2013](http://msdn.microsoft.com/library/6fb4bf41-0846-4dca-ad9e-906afdfd3d2b.aspx) (MSDN)|
|corev15.css|The CSS file that contains most of the main functionality for SharePoint. The following is the default file location:_layouts\15 folder||
|CSSRegistration|A reference in a master page, such as seattle.master, that loads most CSS that is applied to most of the default UI.|Use the  **CSSRegistration** control in a master page to override default CSS.|
|Custom action|Actions you can use to customize and interact with lists and the ribbon on the host web.| [How to: Create custom actions to deploy with apps for SharePoint](http://msdn.microsoft.com/library/bbd11f94-1798-453e-bbb0-e5eb0df8dc75.aspx)|
|Device channels|Render a single SharePoint publishing site in more than one way by using unique channels to target content rendering on specific devices.| [SharePoint 2013 Design Manager device channels](http://msdn.microsoft.com/library/a924bd7b-a5e3-41bf-b0a7-3e43945fa951.aspx)|
|Display templates|Templates used by Search Web Parts to show the results of a query made to the search index.| [SharePoint 2013 Design Manager display templates](http://msdn.microsoft.com/library/1a782bac-48ee-4baf-8751-0f943a306e0f.aspx)|
|Image rendition|Display differently sized versions of an image on a publishing site based on the same source image.| [SharePoint 2013 Design Manager image renditions](http://msdn.microsoft.com/library/d08a74c0-5674-4f26-8646-11ea1f081c85.aspx)|
|Managed metadata| Features in SharePoint that enable you to define terms, term sets, groups, and labels for terms. Sometimes referred to as taxonomy. In SharePoint 2013, the managed metadata system is the foundation for managed navigation.| [Managed metadata and navigation in SharePoint 2013](http://msdn.microsoft.com/library/b66d4ec1-a2ef-49cc-8ca5-a6b516bff02e.aspx)|
|Managed navigation|Navigation for publishing sites that is built based on managed metadata. Navigation is built from a specified term set in the term store. | [Managed navigation in SharePoint 2013](http://msdn.microsoft.com/library/c9da5011-3c73-4b83-8e00-e7a03a71ed02.aspx)|
|Master page|A page that standardizes the behavior and presentation of the left navigation and top navigation areas of a SharePoint page.| [Master pages, the Master Page Gallery, and page layouts in SharePoint 2013](http://msdn.microsoft.com/library/80b9a360-bc2e-46c6-b0ca-1bc487b73db6.aspx)|
|Master Page Gallery|A special document library in SharePoint 2013 where all branding elements-master pages, page layouts, JavaScript files, CSS, and images-are stored by default. Every site has its own Master Page Gallery.When you create custom branding elements, we recommend that you store custom assets in the default Master Page Gallery file structure.| [Master pages, the Master Page Gallery, and page layouts in SharePoint 2013](http://msdn.microsoft.com/library/80b9a360-bc2e-46c6-b0ca-1bc487b73db6.aspx)|
|Oslo.master|A master page in SharePoint 2013.|Moves the current navigation into the same position as the top navigation region. |
|Page content control|A control on a publishing site where a Web Part can be added.||
|Page layout|A template for a SharePoint publishing site page that lets users lay out information on the page in a consistent way.||
|Quick Launch|Manages the navigation elements on the left side of the page of a collaboration site.|You can add heading links to group navigation items.|
|REST|A stateless architectural style that abstracts architectural elements and uses HTTP verbs to read and write data from web pages that contain XML files.| [Get started with the SharePoint 2013 REST service](http://msdn.microsoft.com/library/2de035a0-ac75-43bd-9665-5c5a59c4c590.aspx)|
|Root web|The first web page in a site collection.|The root web is also sometimes referred to as the Web Application Root. |
|Seattle.master|The default .master page for SharePoint 2013 team sites and publishing sites.||
|Site layout|See master page.|The site layout combines the .master page of a theme with its corresponding .preview file.|
|Structured navigation|A navigation structure for publishing sites that is based on the site hierarchy of the publishing site. You can add headers and links to manually replace or customize the structured navigation that SharePoint automatically generates.| [How to: Customize Navigation in SharePoint Server 2010 (ECM)](https://msdn.microsoft.com/en-us/library/office/ms558975%28v=office.14%29.aspx)|
|Theme|A simple way to apply light branding to a SharePoint site. The default file location for themes is the _themes folder of the site.|Themes are an easy way to apply custom branding to SharePoint sites.<br/> [Themes overview for SharePoint 2013](http://msdn.microsoft.com/library/ae585dd3-82fe-46bb-ac93-065edc0a16f4.aspx)<br/> [How to: Deploy a custom theme in SharePoint 2013](http://msdn.microsoft.com/library/f703df24-8e56-4e6a-bc37-95acbb3c83e8.aspx)|
|Theming engine|A set of files and functionality that define the look, feel, behavior, and file associations of composed looks.||
|User Agent String|Information that a browser passes to a website that identifies the software that makes the request from the server.| [SharePoint 2013 Design Manager device channels](http://msdn.microsoft.com/library/a924bd7b-a5e3-41bf-b0a7-3e43945fa951.aspx)|
|User Custom Action|A CSOM property that returns the collection of custom actions for a website, list, or site collection. The default file location is the following: 15\TEMPLATE\FEATURES|[UserCustomAction class](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.usercustomaction.aspx)<br/>[How to: Create custom actions to deploy with apps for SharePoint](http://msdn.microsoft.com/library/bbd11f94-1798-453e-bbb0-e5eb0df8dc75.aspx)<br> [How to: Work with User Custom Actions](https://msdn.microsoft.com/en-us/library/office/ee538686%28v=office.14%29.aspx) [Default Custom Action Locations and IDs](http://msdn.microsoft.com/library/6889686e-f6e6-4da0-bfd4-099878614980.aspx)|

## In this section
<a name="sectionSection1"> </a>



|**Article**|**Shows you how to...**|
|:-----|:-----|
| [Use composed looks to brand SharePoint sites](97342600-351b-4438-9a6f-0d71b926c6d9.md)|Apply composed looks, including colors, fonts, and a background image, to your SharePoint 2013 and SharePoint Online sites.|
| [Use remote provisioning to brand SharePoint pages](fd80f737-3040-4aec-802c-a37a43a7a7c2.md)|Use remote provisioning to interact with themes in SharePoint.|
| [Use CSS to brand SharePoint pages](f1ab3b40-66c9-476f-8bed-957fe67c1353.md)|Use remote provisioning to interact with themes in SharePoint.|
| [Customize a SharePoint page by using remote provisioning and CSS](f39dffb7-4d78-4ba4-82e3-b79847fafc21.md)|Use CSS to customize SharePoint rich text fields and Web Part Zones.|
| [Update the branding of existing SharePoint sites and page regions](f8e1fb38-d4f9-4ea5-911e-d4b7bfd0fa4e.md)|Customize and then refresh the branding of existing sites and regions of a SharePoint page.|

## Additional resources
<a name="bk_addresources"> </a>


-  [Branding and site provisioning solutions for SharePoint 2013 and SharePoint Online](347f4d3d-5657-42da-ae01-3b5aea3a16c7.md)
    
-  [SharePoint pages and the page model](7d1ca7d4-f229-40e8-b2a3-08fb5e113483.md)
    
-  [SharePoint development and design tools and practices](6ec1cef5-05ff-4b6a-9a7b-d02c5e9b30b8.md)
    
