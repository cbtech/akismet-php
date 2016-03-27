# akismet-php
Stop spam from hitting your website comments or content using Akismet.

## Getting Started

First, get your Akismet key (if you still do not have one) from here: https://akismet.com/

Then install the package using Composer and add this line to your ```require``` statement.
```json
"antoniotajuelo/akismet-php": "0.0.1"
```

And finally run from terminal:
```Bash
sudo composer update
```

## Code Sample

```php
use AntonioTajuelo\Akismet\Akismet;

$akismet = new Akismet('YOUR-AKISMET-KEY','YOUR-WEBSITE-URL');

// Check if the Akismet key is valid
$isValidKey = $akismet->verifyKey();
var_dump($isValidKey);

// Check if a comment is spam
$params = [
  'is_test'        => 1,
  'user_ip'        => '123.123.123.123',
  'user_agent'     => 'blablabla',
  'comment_author' => 'viagra-test-123',
  'coment_content' => 'Hello. This is a spam message. Cheap viagra. Sale.'
];
$isSpamComment = $akismet->commentCheck($params);
var_dump($isSpamComment);
```

## Available Methods

### verifyKey()

Checks if your key is valid. Does not take any parameters.

### commentCheck()

This is the call you will make the most. It takes a number of arguments and characteristics about the submitted content and then returns a thumbs up or thumbs down. Performance can drop dramatically if you choose to exclude data points. The more data you send Akismet about each comment, the greater the accuracy. We recommend erring on the side of including too much data.

This method takes an array of parameters containing any amount of the following keys.

Parameter | Required | Description
 --- | --- | --- | ---
```user_ip``` | Yes | IP address of the comment submitter.
```user_agent``` | Yes | User agent string of the web browser submitting the comment - typically the ```HTTP_USER_AGENT``` cgi variable. Not to be confused with the user agent of your Akismet library.
```referrer``` (note spelling) | - | The content of the ```HTTP_REFERER``` header should be sent here.
```permalink``` | - | The permanent location of the entry the comment was submitted to.
```comment_type``` | - | May be blank, comment, trackback, pingback, or a made up value like "registration".
```comment_author``` | - | Name submitted with the comment.
```comment_author_email``` | - | Email address submitted with the comment.
```comment_author_url``` | - | URL submitted with comment.
```comment_content``` | - | The content that was submitted.
```comment_date_gmt``` | - | The UTC timestamp of the creation of the comment, in ISO 8601 format. May be omitted if the comment is sent to the API at the time it is created.
```comment_post_modified_gmt``` | - | The UTC timestamp of the publication time for the post, page or thread on which the comment was posted.
```blog_lang``` | - | Indicates the language(s) in use on the blog or site, in ISO 639-1 format, comma-separated. A site with articles in English and French might use "en, fr_ca".
```blog_charset``` | - | The character encoding for the form values included in comment_* parameters, such as ```UTF-8``` or ```ISO-8859-1```.
```user_role``` | - | The user role of the user who submitted the comment. This is an optional parameter. If you set it to "administrator", Akismet will always return false.
```is_test``` | - | This is an optional parameter. You can use it when submitting test queries to Akismet.

### submitSpam()

This call is for submitting comments that weren't marked as spam but should have been.

It is very important that the values you submit with this call match those of your comment-check calls as closely as possible. In order to learn from its mistakes, Akismet needs to match your missed spam and false positive reports to the original comment-check API calls made when the content was first posted. While it is normal for less information to be available for submit-spam and submit-ham calls (most comment systems and forums will not store all metadata), you should ensure that the values that you do send match those of the original content.

This method takes an array of parameters containing any amount of the following keys.

Parameter | Required | Description
 --- | --- | --- | ---
```user_ip``` | Yes | IP address of the comment submitter.
```user_agent``` | Yes | User agent string of the web browser submitting the comment - typically the ```HTTP_USER_AGENT``` cgi variable. Not to be confused with the user agent of your Akismet library.
```referrer``` (note spelling) | - | The content of the ```HTTP_REFERER``` header should be sent here.
```permalink``` | - | The permanent location of the entry the comment was submitted to.
```comment_type``` | - | May be blank, comment, trackback, pingback, or a made up value like "registration".
```comment_author``` | - | Name submitted with the comment.
```comment_author_email``` | - | Email address submitted with the comment.
```comment_author_url``` | - | URL submitted with comment.
```comment_content``` | - | The content that was submitted.

### submitHam()

This call is intended for the submission of false positives - items that were incorrectly classified as spam by Akismet. It takes identical arguments as comment check and submit spam.

Remember that, as explained in our submit-spam documentation, you should ensure that any values you're passing here match up with the original and corresponding comment-check call.

This method takes an array of parameters containing any amount of the following keys.

Parameter | Required | Description
 --- | --- | --- | ---
```user_ip``` | Yes | IP address of the comment submitter.
```user_agent``` | Yes | User agent string of the web browser submitting the comment - typically the ```HTTP_USER_AGENT``` cgi variable. Not to be confused with the user agent of your Akismet library.
```referrer``` (note spelling) | - | The content of the ```HTTP_REFERER``` header should be sent here.
```permalink``` | - | The permanent location of the entry the comment was submitted to.
```comment_type``` | - | May be blank, comment, trackback, pingback, or a made up value like "registration".
```comment_author``` | - | Name submitted with the comment.
```comment_author_email``` | - | Email address submitted with the comment.
```comment_author_url``` | - | URL submitted with comment.
```comment_content``` | - | The content that was submitted.

## Additional Documentation

Akismet API Documentation: https://akismet.com/development/api/#detailed-docs

## Creator

**Antonio Tajuelo**
- https://github.com/antoniotajuelo
- https://twitter.com/antoniotajuelo