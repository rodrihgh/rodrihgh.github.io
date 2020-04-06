---
permalink: /contact/
title: "Contact"
---

Do you have any questions, comments, suggestions... ?

Please leave them here so that we can get in contact.

<form
  action="https://formspree.io/mrgaproo"
  method="POST"
>
  <fieldset>
  {% if site.reCaptcha.enabled %}
  <div class="g-recaptcha" data-sitekey={{site.reCaptcha.siteKey}}></div>
  {% endif %}
  <legend>Contact Form</legend>
  <label>
    Your email:
    <input type="text" name="_replyto">
  </label><br>
  <label>
    Your message:
    <textarea name="message"></textarea>
  </label><br>
  <button type="submit" class="btn--primary">Send</button>
  </fieldset>
</form>
