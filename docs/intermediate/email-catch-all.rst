.. include:: /_includes/snippets/__ANNOUNCEMENTS__.rst

.. _email_catch_all:

***************
Email catch-all
***************

DreamDevBox Intranet
=================

All your projects can send emails to whatever recipient. You do not have to worry that they will
actually being sent. Each PHP container runs a local postfix mailserver that intercepts
all outgoing mails and puts them into a local mailbox by the user ``dreamdevbox``.

In order to view sent emails open up the dreamdevbox intranet http://localhost/mail.php.
There you can also test email sending and verify that they really stay locally.

.. include:: /_includes/figures/dreamdevbox/dreamdevbox-intranet-emails.rst

In the above image from the intranet you can see that all emails sent to whatever recipient
have been caught by the DreamDevBox and are available to be read.


MailHog
=======

Instead of using the very basic DreamDevBox intranet UI for emails, you can also enable MailHog
and use this to view sent email.s

.. seealso:: :ref:`custom_container_enable_mailhog`
