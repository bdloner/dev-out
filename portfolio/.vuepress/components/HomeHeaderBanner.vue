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

    @import "ms-mixin";

    // Images

    $ms-images-slide-width: 700px;
    $ms-images-slide-height: 400px;

    // Using SCSS mixin to generate the final CSS code for the slider
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
      background-image: url('../portfolio-carousel/img/harvey-gibson-498362-unsplash.jpg');
    }
    &:nth-child(2) .ms-slide__image {
      background-image: url('../portfolio-carousel/img/andre-hunter-461305-unsplash.jpg');
    }
    &:nth-child(3) .ms-slide__image {
      background-image: url('../portfolio-carousel/img/joanna-nix-389128-unsplash.jpg');
    }
    &:nth-child(4) .ms-slide__image {
      background-image: url('../portfolio-carousel/img/jurica-koletic-321003-unsplash.jpg');
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

<script src="../libs/momentum-slider.js"></script>

<script>

// Initializing the images slider
var msImages = new MomentumSlider({
    // Element to append the slider
    el: '.sliders-container',
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
    }
});

// Get pagination items
var pagination = document.querySelector('.pagination');
var paginationItems = [].slice.call(pagination.children);

// Update initialization for images slider
var msImages = new MomentumSlider({
    // MORE OPTIONS

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

</script>