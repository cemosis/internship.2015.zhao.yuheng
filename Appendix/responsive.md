#Responsive Design

The Boostrap 3 natively supports Reponsive Web Design. By using its grid system, we can even explicitly match our layout to different devices.

The grid system has four tiers of classes: xs(phones), sm(tablets), md(desktops), and lg(larger desktops). And a full row of grid is equal to 12. In HTML code, we can in fact use them at the same time, so that the layout would changes as the screen width of the device changes.

```html
<div class="col-xs-12 col-md-6"></div>
	...
<div class="col-xs-12 col-md-6"></div>
	...
```
On desktop device, these two divs are displayed side by side. On smartphones, they are displayed into two lines.

To defines different style rules for a specify element in web page, i used media queries.

```html
@media screen and (max-width: 990px){
  .slide1, .slide2, .slide3, .slide4, .slide5 {
    height: 110vh;
  }
}

@media screen and (max-width: 768px){
  .slide1, .slide2, .slide3, .slide4, .slide5 {
    height: 110vh;
  }
}

@media screen and (max-width: 400px){
  .slide1, .slide2, .slide3, .slide4, .slide5 {
    height: 155vh;
  }
} 

```
This code snippet means that the height of slides grows as the screen width of device becomes smaller. The CSS `vh` unit means its relative to viewport'height or width.
