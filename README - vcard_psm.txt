README - April 9, 2010
Peter McMillan, peterm@ucsc.edu

6/20/2010 - Finalized testing with Chris Gaylord and installed the renamed module into production.

6/17/2010 - Outlook is not importing telephone entries correctly. Using the thread, http://drupal.org/node/282613 we need to use the addParam('TYPE', 'WORK'), addParam('TYPE', 'CELL'), etc., to mark the record properly.


This is a modified version of the Drupal module vCard. It is renamed so that it does not get clobbered in an automatic update.

Here's our modifications:
* module name
* module is patched based on info at, http://drupal.org/node/216583
* In the function vcard_get($uid), I've modified (added) fields to the profile that we want to use in the vCard. These include:
- profile_home_telephone
- profile_mobile_telephone
- profile_assistant
- profile_assistant_phone
- profile_eoc_other_contacts
- profile_title
- profile_revision

* In the function, vcard_fetch($uid), we create an $account variable to hold the data pulled from user_load(). We pass in the uid via URL. user_load(array('uid' => $uid)) grabs all the profile fields for the individual. We really only need First and Last, so there may be a more efficient way to do get that info, but the overhead in user_load on this system is low, so we don't mind being a bit inefficient.

We then create the $download_fullname variable. We set $download_fullname to $account->profile_first . " " . $account_profile_last. 

Finally, in the header() creation, we are able to give a filename of $download_fullname.vcf.

* In the function _vcard_properties, I've added array items for title, note, revision

* The revision field is a date field so we can mark the records with a last date modified stamp.

References
This module is based on the PEAR vcard_build library. There are web resources on google for getting details about more advanced or esoteric vcard properties. I found wikipedia to be helpful on this little project.
http://en.wikipedia.org/wiki/VCard
http://vcardmaker.wackomenace.co.uk/
http://drupal.org/project/vcard