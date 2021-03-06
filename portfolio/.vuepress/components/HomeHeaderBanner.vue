<template>
  <div class="home-header-banner">
    <main class="sliders-container"></main>
    <ul class="pagination">
        <li class="pagination__item"><a class="pagination__button"></a></li>
        <li class="pagination__item"><a class="pagination__button"></a></li>
        <li class="pagination__item"><a class="pagination__button"></a></li>
        <li class="pagination__item"><a class="pagination__button"></a></li>
    </ul>
  </div>
</template>

<style lang="scss" scope>

// SCSS mixin to generate the final CSS code for the slider

// Params:
// - $cssClass: CSS class to match the slider container
// - $slider-length: Length (width or height) of slider container
// - $slider-center: If slider should be centered
// - $slide-width: Fixed width for each slide
// - $slide-height: Fixed height for each slide
// - $vertical: If slider should be vertical (true), or horizontal (false)
// - $reverse: If slider should have reversed slides (first items at the end)
// - $debug: Show helpful background colors to help debugging

@mixin ms(
  // Default values
  $cssClass: 'ms-container',
  $slider-length: 400px,
  $slider-center: false,
  $slide-width: 90px,
  $slide-height: 90px,
  $vertical: false,
  $reverse: false,
  $debug: false
) {
  // Slider container
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
      // Just for testing the current slide position (centered)
      &:before {
        content: '';
        position: absolute;
        background-color: rgba(0, 0, 0, 0.3);
        border-radius: 100%;
      }
    }

    @if $vertical {
      // Styles for vertical slider
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
  
        // Centering slider track vertically, to get current slide centered
        .ms-track {
          flex-direction: column;
          top: calc(50% - #{$slide-height / 2});
        }
  
        @if $reverse {
          // Reverse styles
          &.ms-container--reverse .ms-track {
            flex-direction: column-reverse;
            top: auto;
            bottom: calc(50% - #{$slide-height / 2});
          }
        }
  
        // Positioning slides vertically
        .ms-slide {
          display: flex;
        }
      }
    } @else {
      // Styles for horizontal slider
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
  
        // Centering slider track horizontally, to get current slide centered
        .ms-track {
          left: calc(50% - #{$slide-width / 2});
        }
  
        @if $reverse {
          // Reverse styles
          &.ms-container--reverse .ms-track {
            flex-direction: row-reverse;
            left: auto;
            right: calc(50% - #{$slide-width / 2});
          }
        }
  
        // Positioning slides horizontally
        .ms-slide {
          display: inline-flex;
        }
      }
    }
  
    // Slider track
    .ms-track {
      display: flex;
      position: absolute;
      white-space: nowrap;
      padding: 0;
      margin: 0;
      list-style: none;
    }
  
    // Slides
    .ms-slide {
      align-items: center;
      justify-content: center;
      width: $slide-width;
      height: $slide-height;
      user-select: none;
  
      @if $debug {
        // A testing backgrounds for slides
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


// Images

$ms-images-slide-width: 700px;
$ms-images-slide-height: 400px;

// SCSS mixin to generate the final CSS code for the slider
@include ms(
  $cssClass: 'ms--images', // CSS class to match the slider container
  $slider-length: 100%,    // The slider container will have full width
  $slider-center: false,   // Don't need to center it, as it is full width
  $slide-width: $ms-images-slide-width,   // Fixed width for each slide
  $slide-height: $ms-images-slide-height, // Fixed height for each slide
  $vertical: false, // The slider should be horizontal
  $reverse: false,  // Normal order
  $debug: false     // No debbug backgrounds in production
);

// Custom styles for images slider
.ms--images {
  left: calc(50% - #{$ms-images-slide-width / 2 - 70px});

  &.ms-container--horizontal .ms-track {
    left: -70px;
  }

  // Slides images
  .ms-slide {
    &:nth-child(1) .ms-slide__image {
      background-image: url('https://cdn.jsdelivr.net/gh/lmgonzalves/momentum-slider@3b3037f7f1bab402fe3c75cd20ab18e04e1cbe0b/portfolio-carousel/img/harvey-gibson-498362-unsplash.jpg');
    }
    &:nth-child(2) .ms-slide__image {
      background-image: url('https://cdn.jsdelivr.net/gh/lmgonzalves/momentum-slider@3b3037f7f1bab402fe3c75cd20ab18e04e1cbe0b/portfolio-carousel/img/andre-hunter-461305-unsplash.jpg');
    }
    &:nth-child(3) .ms-slide__image {
      background-image: url('https://cdn.jsdelivr.net/gh/lmgonzalves/momentum-slider@3b3037f7f1bab402fe3c75cd20ab18e04e1cbe0b/portfolio-carousel/img/joanna-nix-389128-unsplash.jpg');
    }
    &:nth-child(4) .ms-slide__image {
      background-image: url('https://cdn.jsdelivr.net/gh/lmgonzalves/momentum-slider@3b3037f7f1bab402fe3c75cd20ab18e04e1cbe0b/portfolio-carousel/img/jurica-koletic-321003-unsplash.jpg');
    }
  }

  .ms-slide__image-container {
    width: 80%;
    height: 80%;
    background-color: rgba(0, 0, 0, 0.3);
    overflow: hidden;
  }

  .ms-slide__image {
    width: 100%;
    height: 100%;
    background-size: cover;
  }
}


// Numbers

$ms-numbers-slide-width: 240px;
$ms-numbers-slide-height: 240px;

@include ms(
  $cssClass: 'ms--numbers',
  $slider-length: $ms-numbers-slide-width,
  $slider-center: false,
  $slide-width: $ms-numbers-slide-width,
  $slide-height: $ms-numbers-slide-height,
  $vertical: false,
  $reverse: false,
  $debug: false
);

.ms--numbers {
  position: absolute;
  left: calc(50% - #{$ms-images-slide-width / 2 + 30px});
  top: calc(50% - #{$ms-images-slide-height / 2 + $ms-numbers-slide-height / 2 - 20px});
  z-index: -1;
  pointer-events: none;

  .ms-slide {
    font-size: 9em;
    font-weight: 900;
    color: rgba(255, 255, 255, 0.2);
  }
}


// Titles

$ms-titles-slide-width: 400px;
$ms-titles-slide-height: 170px;

@include ms(
  $cssClass: 'ms--titles',
  $slider-length: $ms-titles-slide-height,
  $slider-center: false,
  $slide-width: $ms-titles-slide-width,
  $slide-height: $ms-titles-slide-height,
  $vertical: true,
  $reverse: true,
  $debug: false
);

.ms--titles {
  position: absolute;
  left: calc(50% - #{$ms-images-slide-width / 2 + 70px});
  top: calc(50% - #{$ms-titles-slide-height / 2});
  z-index: 1;
  pointer-events: none;

  .ms-track {
    white-space: normal;
  }

  .ms-slide {
    font-size: 3.3em;
    font-weight: 600;

    h3 {
      margin: 0;
      text-shadow: 1px 1px 2px black;
    }
  }
}


// Links

$ms-links-slide-width: 120px;
$ms-links-slide-height: 60px;

@include ms(
  $cssClass: 'ms--links',
  $slider-length: $ms-links-slide-height,
  $slider-center: false,
  $slide-width: $ms-links-slide-width,
  $slide-height: $ms-links-slide-height,
  $vertical: true,
  $reverse: false,
  $debug: false
);

.ms--links {
  position: absolute;
  left: calc(50% - #{$ms-images-slide-width / 2 + 70px});
  top: calc(50% + #{$ms-titles-slide-height / 2 + 20px});
  z-index: 1;

  .ms-track {
    white-space: normal;
  }

  .ms-slide__link {
    font-weight: 600;
    padding: 5px 0 8px;
    border-bottom: 2px solid white;
    cursor: pointer;
  }
}


// Pagination

.pagination {
  display: flex;
  position: absolute;
  left: calc(50% - #{$ms-images-slide-width / 2 + 70px});
  top: calc(100%);
  list-style: none;
  margin: 0;
  padding: 0;
  overflow: hidden;
  z-index: 1;

  &__button {
    display: inline-block;
    position: relative;
    width: 36px;
    height: 20px;
    margin: 0 5px;
    cursor: pointer;
    
    &:before, &:after {
      content: '';
      position: absolute;
      left: 0;
      top: calc(50% - 1px);
      width: 100%;
      box-shadow: 0 1px 0 #0B0D14;
    }
    
    &:before {
      height: 2px;
      background-color: #6A3836;
    }
    
    &:after {
      height: 3px;
      background-color: #DC4540;
      opacity: 0;
      transition: 0.5s opacity;
    }
  }
}

.pagination__item--active .pagination__button:after {
  opacity: 1;
}


// Responsive styles

@media screen and (max-width: 860px) {
  .ms--numbers {
    left: calc(50% - #{$ms-numbers-slide-width / 2});
  }

  .ms--titles {
    left: calc(50% - #{$ms-titles-slide-width / 2});
    top: calc(50% - #{$ms-titles-slide-height / 2 + 50px});
    text-align: center;
  }

  .ms--links {
    left: calc(50% - #{$ms-links-slide-width / 2});
    top: calc(50% + #{$ms-links-slide-height / 2 + 50px});
  }

  .pagination {
    left: 50%;
    top: calc(100% - 50px);
    transform: translateX(-50%);
  }
}

@media screen and (max-width: 600px) {
  .ms--images {
    overflow: visible;
  }
}

@media screen and (max-width: 400px) {
  .ms--titles {
    .ms-slide {
      transform: scale(0.8);
    }
  }
}

</style>

<script>

export default {
  mounted: function() {
    var MomentumSlider = require("momentum-slider");
    
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

  }
}
</script>