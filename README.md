# LESS Module for SilverStripe CMS

Simple wrapper for [lessphp](http://leafo.net/lessphp/) to integrate 
[LESS](http://lesscss.org/) in SilverStripe.

## Thanks

I snagged all the good bits from [Less](https://github.com/axllent/silverstripe-less) 
by [Ralph Slooten](https://github.com/axllent)!

## Features
* Uses [lessphp](http://leafo.net/lessphp/)
* Includes flushing option (?flush=1) to regenerate CSS stylesheets
(ie. force undetected less changes with @import)
* Check all required *.css files for a *.less equivalent, so works transparently.

## Requirements

 * SilverStripe 2 or 3
 * lessphp
 * Webserver read & write permissions to the directories containing
 the *.less files to write compiled css files

## Installation

### Composer

	composer require tardinha/silverstripe-less
	
Install via composer, run dev/build

## Usage

In your Template.ss you can refer to your less files either by name (eg: stylesheet.less) or
stylesheet.css (the parser will check to see if there is a less file for all css files).

<pre>
&lt;% require css(themes/mytheme/css/stylesheet.less) %&gt;
</pre>

or in your Page Controller you could:

### Method 1
<pre>
class Page_Controller extends ContentController {

	public function init() {
		parent::init();
		if ( Director::isDev() ){
			Requirements::css($this-&gt;ThemeDir() . '/css/stylesheet1.less');
			Requirements::css($this-&gt;ThemeDir() . '/css/stylesheet2.less');
			Requirements::css($this-&gt;ThemeDir() . '/css/stylesheet3.less');
		} else {
			/* combined.less simply includes a merged list of the above stylesheets
			 * in the same order as above:
			 * @import "stylesheet1";
			 * @import "stylesheet2";
			 * @import "stylesheet3";
			*/
			Requirements::css($this-&gt;ThemeDir() . '/css/combined.less');
		}
	}

}
</pre>

### Method 2
<pre>
class Page_Controller extends ContentController {

	public function init() {
		parent::init();
		/* The parser will find css/stylesheet[1-3].less files are parse those before combining */
		$css[] = $this-&gt;ThemeDir() . '/css/stylesheet1.css';
		$css[] = $this-&gt;ThemeDir() . '/css/stylesheet2.css';
		$css[] = $this-&gt;ThemeDir() . '/css/stylesheet3.css';
		Requirements::combine_files('combined.css', $css);
		Requirements::process_combined_files();
	}

}
</pre>

