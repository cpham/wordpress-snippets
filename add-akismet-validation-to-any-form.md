#Add Akismet validation to any form

Use this function to validate your form data through Akismet's spam filtering. 
You must have Akismet activated in order to use this.

```
function validate_spam($form_data) {
	
	if (function_exists('akismet_init')) {
		
		$wpcom_api_key = get_option('wordpress_api_key');
		
		if (!empty($wpcom_api_key)) {
		
			global $akismet_api_host, $akismet_api_port;
			
			$fields = array();	
			
			//Set default user information
			$fields['user_ip'] = preg_replace( '/[^0-9., ]/', '', $_SERVER['REMOTE_ADDR'] );
			$fields['user_agent'] = $_SERVER['HTTP_USER_AGENT'];
			$fields['referrer'] = $_SERVER['HTTP_REFERER'];
			$fields['blog'] = get_option('home');
			
			//Map $form_data fields to akismet fields
			$fields['comment_content'] = $form_data['message'];
			$fields['comment_author_email'] = $form_data['email_address'];
			$fields['comment_author'] = $form_data['full_name'];
			$fields['comment_author_url'] = $form_data['website'];
			
			$query_string = http_build_query($fields);
			$response = akismet_http_post($query_string, $akismet_api_host, '/1.1/comment-check', $akismet_api_port);
			
			
			//Spam is detected
			if ($response[1] == 'true') {
				update_option('akismet_spam_count', get_option('akismet_spam_count') + 1);	
				
				$isSpam = TRUE;				
			}
			
			//Spam is not detected
			else {
				$isSpam = FALSE;
			}
									
		}
	}
}
```

##Example usage

###The form
```

<form method="post">
	<p><input type="text" placeholder="Name" name="full_name" /></p>
	<p><input type="email" placeholder="Email" name="email_address" /></p>
	<p><input type="text" placeholder="Website" name="website" /></p>
	<p><textarea name="message" placeholder="Your Message"></textarea></p>
	<p><input type="submit" value="Send Message' /></p>
</form>

```

###Processing the form
```
<?php
  $form_data = $_POST;
  $isSpam = validate_spam($form_data);
  
  if($isSpam) {
    exit('Spam detected');
  }
  else {
    //Spam not detected. Go on processing your form.
  }
?>
```
