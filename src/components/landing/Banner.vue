<template>
  <div id="banner" class="banner container-fluid p-5 align-items-center">
    <div class="container p-md-5">
      <div class="ps-lg-5 row h-50">
        <div class="mt-auto mb-auto mb-md-3 text-lg-start">
          <h1 class="title text-light">
            More Data <green>Less Hassle</green>
          </h1>
        </div>
      </div>
      <div class="ps-lg-5 row h-50">
        <div class="mb-auto text-lg-start">
          <p class="fs-5 lh-lg text-light mb-5">
            View comprehensive, interactive, and free statistical data with
            Property Planners
            <br />
            Powerful charts at your fingertips for <b>FREE</b>
          </p>
          <button type="button" id="signup_banner" class="btn btn-primary rounded-pill btn-lg"
            onclick="location.href='/signup'">
            Sign Up
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
@import url(https://fonts.googleapis.com/css?family=Lato:400,300,100,700,900);

.banner {
  background-image: linear-gradient(0deg,
      rgba(var(--main-dark-rgb), 0.7),
      rgba(var(--main-dark-rgb), 1)),
    url("../../assets/profile_carousel.jpg");
  /* object-fit: cover; */
  background-size: cover;
}

.title {
  font-family: "Lato";
  font-weight: 900;
  font-size: 4rem;
}

#signup_banner {
  width: 120px;
  background-color: var(--main-green);
  border-color: var(--main-green);
  transition: 300ms;
}

#signup_banner:hover {
  /* color: var(--main-green); */
  background-color: rgba(var(--main-green-rgb), 0);
}
</style>