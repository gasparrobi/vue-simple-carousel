### simple carousel in Vue

usage:

```html
<!-- use trim if you don't want padding at the start and end of the carousel -->
<SimpleCarousel :height="200" :trim="true">
  <!-- add padding to slide-wrapper for gap between slides -->
  <div class="slide-wrapper">
    <div class="slide"></div>
  </div>

  <div class="slide-wrapper">
    <div class="slide"></div>
  </div>

  <div class="slide-wrapper">
    <div class="slide"></div>
  </div>
</SimpleCarousel>
```
