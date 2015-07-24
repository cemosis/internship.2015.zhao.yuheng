# Cutomise Boostrap framework

A simple way to cutomise is [here](http://getbootstrap.com/customize/). We choose features and components that what we want.
The motive to do so is that I need the navbar collapses at "the right point". Because by deault it collapses when the device's screen width equals to 768px. This is fine for most cases, but in our case there're 3 logos at the left of navigation bar. So the screen width reduces to around 992px, the `navbar-collapse` overlaps a part of logos.

The less variable specifies the collapse break point is called `@grid-float-breakpoint` in Boostrap 3. In the cutomise web page I change that variable to `@screen-md` which equals to 992px. It represents the minimum screen width of desktop device.
 
# Fix logo links

We put 3 logo links with icons at the left of navigation bar. But the problem is, even they are all put in the `navbar-header` div tag. Still, the `navbar-collapse` covers all the links. So the user can see all three logos but can't click their links.
This is because the `navbar-collapse` has 100% width and we cannot simply change it. 

To resolve this problem, I firstly put 3 logos outside `navbar-header` div tag and defines them to a class called desktop-brand.
Then I put 3 same logos inside `navbar-header` and called them mobile-brand.

```html
<div class="navbar-header">
        <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#main-navbar" aria-expanded="false">
        <span class="sr-only">Toggle navigation</span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        </button>

        <!-- mobile logos -->
        <a class="mobile-brand navbar-brand" href="http://www.unistra.fr/">
          <img alt="Unistra" src="http://www.unistra.fr/fileadmin/templates/unistra-v2/images/logo.png" style="height:30px">
        </a>
        
        <a class="mobile-brand navbar-brand" href="http://www.cemosis.fr">
          <img alt="Cemosis" src="http://www.google.com/a/cpanel/cemosis.fr/images/logo.gif?service=google_default" style="height:30px">
        </a>
        <a class="mobile-brand navbar-brand" href="https://mathinfo.unistra.fr/">
          <img alt="UFR" src="https://clarinet.u-strasbg.fr/~gallais/uploads/Site//ufr.png" style="height:30px">
        </a>

      </div>
      {% assign sorted_promotions = (site.data.promotions.promotions | sort: 'year' | reverse) %}
      <!-- Navbar buttons -->
      <div class="collapse navbar-collapse navbar-fixed-top" id="main-navbar">

        <!-- logos -->
        <a class="desktop-brand navbar-brand" href="http://www.unistra.fr/">
          <img alt="Unistra" src="http://www.unistra.fr/fileadmin/templates/unistra-v2/images/logo.png" style="height:30px">
        </a>
        
        <a class="desktop-brand navbar-brand" href="http://www.cemosis.fr">
          <img alt="Cemosis" src="http://www.google.com/a/cpanel/cemosis.fr/images/logo.gif?service=google_default" style="height:30px">
        </a>
        <a class="desktop-brand navbar-brand" href="https://mathinfo.unistra.fr/">
          <img alt="UFR" src="https://clarinet.u-strasbg.fr/~gallais/uploads/Site//ufr.png" style="height:30px">
        </a>
```

Now let media queries control them and display them when it's the right time. When screen width is smaller that 992px, it displays the mobile-brand inside the `navbar-header` and the `navbar-collapse` is also collapsed(because I changed the break point variable).

```css
@media screen and (min-width: 992px){
  .mobile-brand {
    display: none;
  }
}
@media screen and (max-width: 992px){
  .desktop-brand {
    display: none;
  }
}
```