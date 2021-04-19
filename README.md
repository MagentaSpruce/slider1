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

Will start by applying an event handler to a keyboard event so the slider can be slid through using the left and right arrow keys.
```JavaScript
document.addEventListener("keydown", function (e) {
    if(e.key === 'ArrowLeft') prevSlide();
    e.key === 'ArrowRight' && nextSlide();
});
```

Now the dots. . .
There is an HTML div for the dots with class dots which is empty. Start by selecting that element and then creating the dots which will fill it. The dots are using the data-slide attribute with the number of the slide that clicking the button will go to. 

Now create one element for each of the slides by looping over them and using a forEach() loop using insertAdjacentHTML beforeend which adds the eement as the last child. The index value will be read dynamically and move to that slide depending on when its corresponding dot is clicked. 
```JavaScript
const dotContainer = document.querySelector(".dots");

const createDots = function () {
    slides.forEach(function (_, i){
        dotContainer.insertAdjacentHTML('beforeend', `<button class="dots__dot" data-slide="${i}"></button>`)
    })
};
createDots();
```

The dots are now formed on the bottom of the slider. Now to make them work by adding another event handler. We will use event delegation so that we will not have to attach an event handler to each dot but instead to the parent of all which is the dot container where the dots were added. Make note to recall that all custom data attributes (like data-slide) are in the dataset.
```JavaScript
dotContainer.addEventListener("click", function (e) {
  if (e.target.classList.contains("dots__dot")) {
    const { slide } = e.target.dataset;
    goToSlide(slide);
  }
});
```

Now need to show which is the current slide by giving the active dot its own class. To activate one first will deactivate all each time a slide is selected then readd it to the active slide. 
```JavaScript
const activateDot = function (slide) {
  document
    .querySelectorAll(".dots__dot")
    .forEach((dot) => dot.classList.remove("dots__dot--active"));
    
        document.querySelector(`.dots__dot[data-slide="${slide}"]`).classList.add('dots__dot--active');
};
```

That removes all the active classes. Now to add on the one that is currently active is done using the data-attribute tag. To select the active slide we use the data-attribute which is selected on using a selector [data-slide="${slide}] which will select the dot. Once selected then adding the active classlist is done.  activateDot(slide); must be added to the cur slide, prev slide, and dots container.
```JavaScript
const activateDot = function (slide) {
  document
    .querySelectorAll(".dots__dot")
    .forEach((dot) => dot.classList.remove("dots__dot--active"));
    
        document.querySelector(`.dots__dot[data-slide="${slide}"]`).classList.add('dots__dot--active');
};
```

The last issue is that on page reload none of the slides are active. So the activateDot() function needs to be called with 0 as the argument.

***End walkthrough
