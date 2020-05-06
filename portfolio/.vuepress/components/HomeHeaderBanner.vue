<template>
  <div class="home-header-banner">
    <!-- Container for all sliders, and pagination -->
    <main class="sliders-container">
        <!-- Here will be injected sliders for images, numbers, titles and links -->

        <!-- Simple pagination for the slider -->
  </div>
</template>

<style lang="scss">
    @mixin ms(
        $cssClass: 'ms-container',
        $slider-length: 400px,
        $slider-center: false,
        $slide-width: 90px,
        $slide-height: 90px,
        $vertical: false,
        $reverse: false,
        $debug: false
    ) {

  .#{$cssClass} {
    position: relative;
    @if $slider-center {
      margin: 0 auto;
    }
    overflow: hidden;
    @if $debug {
      background-color: rgba(0, 0, 0, 0.2);
    }

    @if $debug {

      &:before {
        content: '';
        position: absolute;
        background-color: rgba(0, 0, 0, 0.3);
        border-radius: 100%;
      }
    }

    @if $vertical {

      &.ms-container--vertical {
        width: $slide-width;
        height: $slider-length;
        max-height: 100%;
  
        @if $debug {
          &:before {
            left: 0;
            top: calc(50% - #{$slide-height / 2});
            width: 100%;
            height: $slide-height;
          }
        }
  
        .ms-track {
          flex-direction: column;
          top: calc(50% - #{$slide-height / 2});
        }
  
        @if $reverse {
          &.ms-container--reverse .ms-track {
            flex-direction: column-reverse;
            top: auto;
            bottom: calc(50% - #{$slide-height / 2});
          }
        }
  
        .ms-slide {
          display: flex;
        }
      }
    } @else {

      &.ms-container--horizontal {
        width: $slider-length;
        height: $slide-height;
        max-width: 100%;
  
        @if $debug {
          &:before {
            left: calc(50% - #{$slide-width / 2});
            top: 0;
            width: $slide-width;
            height: 100%;
          }
        }
  
        .ms-track {
          left: calc(50% - #{$slide-width / 2});
        }
  
        @if $reverse {
          &.ms-container--reverse .ms-track {
            flex-direction: row-reverse;
            left: auto;
            right: calc(50% - #{$slide-width / 2});
          }
        }
  
        .ms-slide {
          display: inline-flex;
        }
      }
    }
  
    .ms-track {
      display: flex;
      position: absolute;
      white-space: nowrap;
      padding: 0;
      margin: 0;
      list-style: none;
    }
  
    .ms-slide {
      align-items: center;
      justify-content: center;
      width: $slide-width;
      height: $slide-height;
      user-select: none;
  
      @if $debug {
        &:nth-child(2n) {
          background-color: rgba(0, 0, 0, 0.1);
        }
        &:nth-child(2n + 1) {
          background-color: rgba(0, 0, 0, 0.2);
        }
      }
    }
  }
}
</style>

<script>

(function() {

    var slidersContainer = document.querySelector('.sliders-container');
    // Initializing the numbers slider
    var msNumbers = new MomentumSlider({
        el: slidersContainer,
        cssClass: 'ms--numbers',
        range: [1, 4],
        rangeContent: function (i) {
            return '0' + i;
        },
        style: {
            transform: [{scale: [0.4, 1]}],
            opacity: [0, 1]
        },
        interactive: false
    });

    // Initializing the titles slider
    var titles = [
        'King of the Ring Fight',
        'Sound of Streets',
        'Urban Fashion',
        'Windy Sunset'
    ];
    var msTitles = new MomentumSlider({
        el: slidersContainer,
        cssClass: 'ms--titles',
        range: [0, 3],
        rangeContent: function (i) {
            return '<h3>'+ titles[i] +'</h3>';
        },
        vertical: true,
        reverse: true,
        style: {
            opacity: [0, 1]
        },
        interactive: false
    });

    // Initializing the links slider
    var msLinks = new MomentumSlider({
        el: slidersContainer,
        cssClass: 'ms--links',
        range: [0, 3],
        rangeContent: function () {
            return '<a class="ms-slide__link">View Case</a>';
        },
        vertical: true,
        interactive: false
    });

    // Get pagination items
    var pagination = document.querySelector('.pagination');
    var paginationItems = [].slice.call(pagination.children);

    // Initializing the images slider
    var msImages = new MomentumSlider({
        // Element to append the slider
        el: slidersContainer,
        // CSS class to reference the slider
        cssClass: 'ms--images',
        // Generate the 4 slides required
        range: [0, 3],
        rangeContent: function () {
            return '<div class="ms-slide__image-container"><div class="ms-slide__image"></div></div>';
        },
        // Syncronize the other sliders
        sync: [msNumbers, msTitles, msLinks],
        // Styles to interpolate as we move the slider
        style: {
            '.ms-slide__image': {
                transform: [{scale: [1.5, 1]}]
            }
        },
        // Update pagination if slider change
        change: function(newIndex, oldIndex) {
            if (typeof oldIndex !== 'undefined') {
                paginationItems[oldIndex].classList.remove('pagination__item--active');
            }
            paginationItems[newIndex].classList.add('pagination__item--active');
        }
    });

    // Select corresponding slider item when a pagination button is clicked
    pagination.addEventListener('click', function(e) {
        if (e.target.matches('.pagination__button')) {
            var index = paginationItems.indexOf(e.target.parentNode);
            msImages.select(index);
        }
    });

})();

</script>