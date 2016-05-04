---
layout: post
title:  "How to replace ActionLink with image using Razor in ASP.NET MVC"
date:   2016-04-08 13:06:04
author: Carlos Liu
categories: asp.net mvc
---

Sometimes you will use **ActionLink** in the page, but the problem is that the view may look a little bit poor when the link's text is long, so at the moment it is better to replace the text by using an image. Below is the steps on how to achieve this


### Create an Extension Method

{% highlight csharp %}
namespace Test.Extensions
{
  /// <summary>
  /// Extensions for HtmlHelper class
  /// </summary>
  public static class HtmlHelperExtensions
  {
	/// <summary>
	/// Replace the action link with image using Razor
	/// </summary>
	/// <param name="html"></param>
	/// <param name="action">The name of the corresponding action.</param>
	/// <param name="routeValues">An object that contains the parameters for a route</param>
	/// <param name="imagePath">The path of the image file</param>
	/// <param name="alt">The alt attribute specifies an alternate text 
	/// for an image, if the image cannot be displayed</param>
	/// <param name="tooltip">Tooltip of the image</param>
	/// <returns>An anchor element (a element) with 'img' tag embedded.</returns>
	public static MvcHtmlString ActionImage(
		this HtmlHelper html, 
		string action, 
		object routeValues, 
		string imagePath, 
		string alt, 
		string tooltip = "")
	{
	  // refer to http://stackoverflow.com/questions/4896439/action-image-mvc3-razor
	  var url = new UrlHelper(html.ViewContext.RequestContext);

	  // build the <img> tag
	  var imgBuilder = new TagBuilder("img");
	  imgBuilder.MergeAttribute("src", url.Content(imagePath));
	  imgBuilder.MergeAttribute("alt", alt);
	  imgBuilder.MergeAttribute("title", tooltip);
	  string imgHtml = imgBuilder.ToString(TagRenderMode.SelfClosing);

	  // build the <a> tag
	  var anchorBuilder = new TagBuilder("a");
	  anchorBuilder.MergeAttribute("href", url.Action(action, routeValues));
	  anchorBuilder.InnerHtml = imgHtml; // include the <img> tag inside
	  string anchorHtml = anchorBuilder.ToString(TagRenderMode.Normal);

	  return MvcHtmlString.Create(anchorHtml);
	}
  }
}

{% endhighlight %}

### Update The Web.config File
If you intent to use the above method in multiple views, then do as below

In the "web.config" file in **Views** directory of your project (notice it is not the main web.config in project's root), find this section:

{% highlight xml %}
<system.web.webPages.razor>
  <pages pageBaseType="System.Web.Mvc.WebViewPage">
	<namespaces>
	  <add namespace="System.Web.Mvc" />
	  <add namespace="System.Web.Mvc.Ajax" />
	  .
	  .
	  <!-- etc -->
	</namespaces>
  </pages>
</system.web.webPages.razor>
{% endhighlight %}

you can add your custom namespace like this:

{% highlight csharp %}
<add namespace="Test.Extensions" />
{% endhighlight %}

that will add the namespace to all of .cshtml (and/or .vbhtml) files.

If you just want to use above method in a single page, you could simplely using the namespace at the top of your view

{% highlight csharp %}
@using Test.Extensions
{% endhighlight %}

### Apply in The View

{% highlight csharp %}
@Html.ActionImage("EditAccount", new { userId = user.Id }, "~/Content/Images/editAccount.png", "Edit", "Edit the account")
{% endhighlight %}

### Other Tips

* \+ It's better to use PNG or JPG image instead of BMP file
* \+ Try to use application relative paths (```~/Content```). The  path `../../Content` might not be valid from different routes (e.g. `/`, `/Home`, `/Home/Index`)

***

## References

> http://stackoverflow.com/questions/4896439/action-image-mvc3-razor
> http://stackoverflow.com/questions/11716916/tooltip-on-image
> http://stackoverflow.com/questions/3239006/how-do-i-import-a-namespace-in-razor-view-page/6723046#6723046
> http://www.w3schools.com/tags/att_img_alt.asp