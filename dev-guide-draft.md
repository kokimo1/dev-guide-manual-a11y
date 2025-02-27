# Dev's guide for manual accessibility checks

This *accessibility checking guide* for developers (last updated 2025-02-27) explains what to look for when conducting manual accessibility tests in a web browser, which will reveal a significant amount of WCAG issues that [automated testing](https://a11y4developers.uic.edu/how-to-test/automated-testing/) cannot detect. This document links to other resources for more granular information. To learn more about accessibility from a developer's perspective, please consult [a11y4developers.uic.edu](https://a11y4developers.uic.edu/). 

1. ***Keyboard testing***  
  Using only your keyboard, navigate through all interactive elements on the page [with the 9 standard input keys](https://a11y4developers.uic.edu/how-to-test/keyboard-testing/#method). Your web component's purpose determines [what keyboard users expect](https://a11y4developers.uic.edu/how-to-test/keyboard-testing/#expected-behavior-keyboard), so consider what your users will experience when interacting with your creation. Generally speaking, good keyboard accessibility also helps avoid many related screen reader issues, so it's recommended to test keyboard-only performance first.  
    🔎**Look for the following**🔍

    - **All conditional failures** of [expected behavior for keyboards](https://a11y4developers.uic.edu/how-to-test/keyboard-testing/#expected-behavior-keyboard): if you press the expected (1) *key*, then the expected (2) *action* must occur, then the (3) *focus* indicator must move to the expected location.
    - **Use cases where mouse interaction doesn't have a keyboard equivalent**. If the thing is clickable, it must be accessible via keyboard.
    - **Illogical reading order** when tabbing through interactive elements.
    - **Focus indicator issues.** After manual interaction, your browser's built-in focus indicator should appear as a complete, consistent blue outline around all sides of the expected location. E.g., ⭐ you press the tab key, the focus indicator should move to the next focusable element.
    - **Anything related to input or focus that is confusing or unexpected.**

2. ***Screen reader testing***  
  Navigate forward & backwards through the page and **test all page elements** and **screen reader "announcements"** using screen reader software such as [NVDA on Windows](https://www.nvaccess.org/download/) (free & open source), [Apple VoiceOver on MacOS & iOS](https://support.apple.com/guide/voiceover-guide/welcome/web) (free app native to MacOS & iOS, see our [Voiceover "how to" section on the a11y4developers site](https://a11y4developers.uic.edu/how-to-test/screen-reader-testing/#method)), or [Orca on Linux](https://orca.gnome.org/) (free & open source, built for the GNOME desktop environment). [Screen reader users expect certain behavior](https://a11y4developers.uic.edu/how-to-test/screen-reader-testing/#expected-behavior-screen-reader), which depends on your component's intended purpose.  
    🔎**Look for the following**🔍

    - **All conditional failures** of [expected behavior for screen readers](https://a11y4developers.uic.edu/how-to-test/screen-reader-testing/#expected-behavior-screen-reader): if you press the expected (1) *key*, then the expected (2) *action* must occur, then the (3) *focus* indicator must move to the expected location, and the screen reader must make the expected (4) *announcement*, which must communicate the element's role, state, label, and/or content.
    - **Elements not identified or "announced" properly.** The screen reader should announce everything on the page except for decorative elements.
    - **Interactive elements that can't be activated.** If the thing is clickable, you must be able to activate it with screen reader software.
    - **Interactive elements that don't announce state change** when their activation changes page content. E.g., ⭐ if activating a closed accordion panel button, the screen reader should announce something like "button, expanded" immediately upon activation. 
    - **Redundancy** in page content, especially labels and heading elements.
    - **Focus indicator issues.** After manual interaction, the screen reader's focus indicator should appear at the expected location. E.g., ⭐ if you press CTRL OPTION RIGHT in Voiceover, the screen reader's focus should move to the next element in the reading order.

3. ***Manual code inspection***  
  After keyboard and screen reader testing, inspect your code for a few more contextual issues that automated scans are unable to completely address.  
    🔎**Look for the following**🔍   

    - **Inappropriate use of semantic markup and ARIA roles.** Ensure HTML elements are used properly to render the content's structure and purpose and do not contain redundant or conflicting aria roles. E.g., ⭐ these two examples intended for button usage are incorrect: `<button role="button" class="heading-button">` and  `<button role="heading" class="heading-button">`.  
    ***In contrast***, this element: `<div role="button" class="heading-button">` uses ARIA appropriately and should only be used as a button. Please note that while this button example does use the correct ARIA role, it still requires scripting to make it as keyboard accessible as the native button element. In general, [native semantic HTML elements are preferable and easier to maintain due to their inherent accessibility features](https://a11y4developers.uic.edu/topics/accessibility-concepts-and-standards/#semantics). Block-level `<div>` and inline `<span>` elements are generic containers for organizing content and should only be used for interactivity if no other options exist. [If you use ARIA attributes in your code, be sure to use them properly as recommended by W3C](https://www.w3.org/WAI/ARIA/apg/). 

    - **Incorrect ARIA mechanism or container for announcing dynamic content changes.** Please note, there are two different types of ARIA roles for this: (1) [Live region roles](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles#4._live_region_roles) (alert, log, marquee, status, timer) and (2) [Window roles](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles#5._window_roles) (dialog, alertdialog), which are all intended for use in specific contexts. E.g., ⭐ if a critical error or security timeout occurs, it should be announced immediately using the alert role. 

