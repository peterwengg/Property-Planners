
<template>
<div class="container-fluid p-0">
  <footer class="p-5">
    <div class="row text-end justify-content-end">
      <div class="col-6 col-md-2 mb-3">
        <!-- <h5>Section</h5> -->
        <ul class="nav flex-column">
          <li class="nav-item mb-2"><a href="#banner" class="nav-link p-0 ">Home</a></li>
          <li class="nav-item mb-2"><a href="#about-us-container" class="nav-link p-0 ">About Us</a></li>
          <li class="nav-item mb-2"><a href="#api-info" class="nav-link p-0 ">Features</a></li>
          <li class="nav-item mb-2"><a href="#testimonials" class="nav-link p-0 ">Testimonials</a></li>
          <li class="nav-item mb-2"><a href="/login" class="nav-link p-0 ">Login</a></li>
          <!-- <li class="nav-item mb-2"><a href="#" class="nav-link p-0 ">About</a></li> -->
        </ul>
      </div>

    </div>
    <div id="line" class="d-flex flex-column flex-sm-row justify-content-between py-4 my-4">
      <p>© 2022 PropertyPlanners, Inc. All rights reserved.</p>
      <ul class="list-unstyled d-flex justify-content-center">
        <li class="ms-3"><a href="https://twitter.com/" target="_blank"><font-awesome-icon icon="fa-brands fa-twitter" size="lg"/></a></li>
        <li class="ms-3"><a href="https://www.instagram.com/" target="_blank"><font-awesome-icon icon="fa-brands fa-instagram" size="lg"/></a></li>
        <li class="ms-3"><a href="https://www.linkedin.com/in/joey-tan-zuyi" target="_blank"><font-awesome-icon icon="fa-brands fa-linkedin" size="lg"/></a></li>
      </ul>
    </div>
  </footer>
</div>
</template>

<style>
footer{
  background-color: var(--main-white);
}
footer h5{
  color: var(--main-dark-green);
}
footer li a,footer p{
  color: rgba(var(--main-dark-green-rgb),0.7) !important
}
footer,#line{
  border-top: 1px solid rgba(var(--main-dark-green-rgb),0.5); 
}
</style>