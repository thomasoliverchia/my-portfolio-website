<template>
<h1 class="text-center my-4 pt-5" id="contact">Contact</h1>
  <div class="contact-section d-flex justify-content-center">
    <div class="row w-100 justify-content-center">
      <div class="col-12 col-md-6">
        <form @submit.prevent="submitForm" class="p-4 border rounded shadow-sm bg-white">
          <div class="mb-3">
            <input type="text" class="form-control contact-form-control" placeholder="First Name M.I. Last Name" v-model="name">
          </div>
          <div class="mb-3">
            <input type="email" class="form-control contact-form-control" placeholder="Email" v-model="email">
          </div>
          <div class="mb-3">
            <textarea class="form-control contact-form-control" rows="6" placeholder="Message" v-model="message"></textarea>
          </div>
          <div class="form-footer d-flex justify-content-between align-items-center">
            <div class="social-icons">
              <a href="https://www.linkedin.com/" id="linkedin" class="me-2"><i class="fab fa-linkedin"></i></a>
              <a href="https://gitlab.com/" id="gitlab" class="me-2"><i class="fab fa-gitlab"></i></a>
              <a href="https://github.com/" id="github"><i class="fab fa-github"></i></a>
            </div>
            <button type="submit" class="submit-btn px-4">Submit</button>
          </div>

                    <!-- recaptcha checkbox -->
                    <div class="d-flex justify-content-end mt-2">
                        <div ref="recaptchaContainer"></div>
                    </div>
					
					<!-- recaptcha checkbox -->
                    <div class="d-flex justify-content-end mt-2">
                        <div ref="recaptchaContainer"></div>
                    </div>
                    
                </form>
                
            </div>
        </div>
    </div>
</template>

<script setup>

	import {ref, onMounted, onBeforeUnmount} from "vue";

	import {Notyf} from "notyf";
	import "notyf/notyf.min.css";

	const notyf = new Notyf();

	const WEB3FORMS_ACCESS_KEY = "f0246589-b32b-48c8-ad47-9f13b2e1a537";

	// email subject
	const subject = "New message from Portfolio Contact form";

	const name = ref("");
	const email = ref("");
	const message = ref("");

	const isLoading = ref(false);

	const submitForm = async()=> {

		// Check if reCAPTCHA token is present, return an error when not verified
		if(!recaptchaToken.value){
			notyf.error("Please verify that you are not a robot")
			return;
		}

		isLoading.value = true;


		try{
			const response = await fetch("https://api.web3forms.com/submit", {
				method: "POST",
				headers: {
					"Content-Type" : "application/json",
					Accept: "application/json"
				},
				body: JSON.stringify({
					access_key: WEB3FORMS_ACCESS_KEY,
					subject: subject,
					name: name.value,
					email: email.value,
					message: message.value
				})
			})

			const result = await response.json();

			if(result.success){
				console.log(result);
				isLoading.value = false;
				notyf.success("Message Sent!");
			}
			else{
				isLoading.value = false;
				notyf.error("Failed to send message");
			}
		}
		catch(error){
			console.log(error);
			isLoading.value = false;
			notyf.error("Failed to send message");
		}
		

	}


	const SITE_KEY = '6Ld-EYAsAAAAALCc7mWidenyOZ-da_9wBTarneft';  

	const recaptchaContainer = ref(null);
	const recaptchaWidgetId = ref(null);
	const recaptchaToken = ref('');

	// Callback called by reCAPTCHA when successful
	function onRecaptchaSuccess(token) {
	  recaptchaToken.value = token;
	}

	// Callback when expired
	function onRecaptchaExpired() {
	  recaptchaToken.value = '';
	}

	// Function to render the reCAPTCHA widget
	function renderRecaptcha() {
	  if (!window.grecaptcha) {
	    console.error('reCAPTCHA not loaded');
	    return;
	  }

	  recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
	    sitekey: SITE_KEY,
	    size: 'normal', // or 'compact'
	    callback: onRecaptchaSuccess,
	    'expired-callback': onRecaptchaExpired,
	  });
	}

	// Function to reset reCAPTCHA 
	function resetRecaptcha() {
	  if (recaptchaWidgetId.value !== null) {
	    window.grecaptcha.reset(recaptchaWidgetId.value);
	    recaptchaToken.value = '';
	  }
	}



	onMounted(() => {
	  // This code waits for the Google reCAPTCHA library to load, then renders the reCAPTCHA widget using onMounted hook. 
	  // The widget is rendered with grecaptcha.render(), which requires a sitekey. 
	  // Callback functions handle success and expiration events. 
	  // reCAPTCHA is reset upon form submission to clear the token.
	  const interval = setInterval(() => {
	    if (window.grecaptcha && window.grecaptcha.render) {
	      renderRecaptcha();
	      clearInterval(interval);
	    }
	  }, 100);

	  onBeforeUnmount(() => {
	    clearInterval(interval);
	  });
	});

	


</script>
