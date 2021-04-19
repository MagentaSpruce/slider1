# slider1
HTML, CSS, and JavaScript slider box component

This is a more advanced slider component which can be utilized across pages and sites as needed with changes and edits done per the instance.

This project helped me to better learn and practice:
1) Nesting
2) Making custom CSS elements
3) Use case of the content property
4) CSS Grid/FlexBox
5) Creating and implementing logic
6) Refactoring code

There are no known bugs with this project. It can be utitilized as is or changed as desired.

A general walkthrough of the JavaScript code is below:
Slider content(slides) start out stacked on top of eachother. To make them side by side the transform: translateX() property is adjusted. This is also what is responsible for moving them.

Starting with a selection.
```JavaScript
const slides = document.querySelectorAll('.slide');
```

Next the slides need to be place side-by-side. 1st slide should be at 0%. 2nd slide at 100%. 3rd slide at 200%. To calculate the percent values a template literal is used to multiply 100 by the index.
```JavaScript
slides.forEach((s, i) => s.style.transform = `translateX(${100 * i)%`));
```

To make the slider work the two arrow buttons need to be selected for and add event listeners added, starting with the right side.
```JavaScript
const btnLeft = document.querySelector('.slider__btn--left');
const btnRight = document.querySelector('.slider__btn--right');

btnRight.addEventListener('click', function(){});
```

Next a new variable is created for the current slide being displayed. The curSlide starts at 0 and when the next slide is selected for then the curSlide is increased by 1. The slides transform percentages also must be changed to match with their new positions. 1st slide is at -100% then 0 then 100% for slide 3. By taking the index and subtracting the current slide  before multiplying that result by 100% to generate the correct transform.
```JavaScript
let curSlide = 0;
btnRight.addEventListener('click', function(){
  curSlide++;
  
  slides.forEach((s, i) => s.style.transform = `translateX(${100 * (i - curSlide)}%)`)
  );
});
```

At this point the right button is working but the slides keep going to the right as the transform values continue to increase. A stop must be applied. This is done by defining the number of slides and then making it stop once that slide is reached.
```JavaScript
const maxSlide = slides.length;

btnRight.addEventListener('click', function(){
  if(curSlide === maxSlide - 1) {
  curSlide = 0;
  } else{
  curSlide++;
  }
  slides.forEach((s, i) => s.style.transform = `translateX(${100 * (i - curSlide)}%)`)
  );
});
```

At this point there is a growing amount of repeat code which will be refactored.
```JavaScript
const goToSlide = function(slide){
    slides.forEach((s, i) => s.style.transform = `translateX(${100 * (i - slide)}%)`)
  );
  goToSlide(0);
  
  const nextSlide = function(){
      if(curSlide === maxSlide - 1) {
  curSlide = 0;
  } else{
  curSlide++;
  }
  goToSlide(curSlide);
  }
  
  btnRight.addEventListener('click', nextSlide);
```

The left button is connected next.
```JavaScript
const prevSlide = function(){
curSlide--;
goToSlide(curSlide)
}

btnLeft.addEventListener('click', prevSlide);
```

Preventing the slide from going too far left (like it went right before) is fixed next.
```JavaScript
const prevSlide = function(){
if(curSlide === 0 {
curSlide == maxSlide - 1;
} else {
  curSlide--;
  }
goToSlide(curSlide)
};
```

At this point the slider is working. It is still missing the keyboard movement and the dot movers which are done next.
