# Overview #

A PHP script which extracts strings from Javascript code and merges with an existing .po file. Then you can translate strings using Poedit.

Includes a small script that converts .po files into JSON so you can include it as an external file in your web application and invoke a function that translates.

The script is smart enough to extract strings excluding regular expressions and comments.

# Usage #

Open Poedit, go to preferences and add a new parser. The command must point to the script "jsgettext.php". Eg: `php /home/www/projects/jsgettext/jsgettext.php -o %o -k "%K" %F`

Create a new catalog and set the base folder which contains the Javascript files you want to parse. Proceed to update from source, translate and save file.

Exec the following command (check the command location and update path):
```
/*
Options:
-i input file name
-o output file name
-k javascript global var name which holds the object with the translations.
*/
php /home/www/projects/jsgettext/po2json.php -i catalog.po -o l10n.js -k l10n
```

From JS:
```
function _(s) {
    return typeof l10n[s] != 'undefined' ? l10n[s] : s;
}
alert(_("Hello world"))
```

## Example source file ##
I could successfully extract the correct strings from the following source file:
```
function _(s) {
	return typeof l10n[s] != 'undefined' ? l10n[s] : s;
}
function test(param) {
	var a = _("Hello world, testing jsgettext");
	func(_('Test string'));
	var reg1 = /"[a-z]+"/i;
	var reg2 = /[a-z]+\+\/"aa"/i;
	var s1 = _('string 1: single quotes');
	var s2 = _("string 2: double quotes");
	var s3 = _("/* comment in string */");
	var s4 = _("regexp in string: /[a-z]+/i");
	var s5 = jsgettext( "another function" );
	var s6 = avoidme("should not see me!");
	var s7 = _("string 2: \"escaped double quotes\"");
	var s8 = _('string 2: \'escaped single quotes\'');

	// "string in comment"
	//;

	/**
	 * multiple
	 * lines
	 * comment
	 * _("Hello world from comment")
	 */
}
```

## TODO ##
  * Only tested with UTF-8, allow to configure source code charset.
  * Poedit parser not tested with plural forms (I never found it useful...)
  * Try to parse a .po file on the fly with Javascript instead of converting it to JSON (is that a really good option?)