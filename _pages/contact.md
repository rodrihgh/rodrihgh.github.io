---
permalink: /contact/
title: "Contact"
recaptcha: true
---

<i class="fas fa-info-circle"></i>Do you have any questions, comments, suggestions... ? Please leave them here so that I can get in contact with you.
{: .notice--primary}

<form
  action="https://formspree.io/mrgaproo"
  method="POST"
>
  <fieldset>
  <div class="g-recaptcha" data-sitekey="{{site.reCaptcha.siteKey}}">
  </div> 
  <legend>Contact Form</legend>
  <label>
    Your email:
    <input type="text" name="_replyto">
  </label><br>
  <label>
    Your message:
    <textarea name="message" rows="5"></textarea>
  </label><br>
  <button type="submit" class="btn btn--primary">Send</button>
  </fieldset>
</form>
