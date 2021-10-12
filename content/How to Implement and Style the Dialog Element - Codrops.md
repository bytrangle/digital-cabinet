# How to Implement and Style the Dialog Element - Codrops
![](https://i7x7p5b7.stackpathcdn.com/codrops/wp-content/uploads/2021/10/Dialog_featured.jpg)

From our sponsor: [Ship fast and never break a thing with Shortcut (formerly Clubhouse.io).](https://srv.buysellads.com/ads/long/x/TH4TNQS3TTTTTT6TFWEN4TTTTTTTR3JTK6TTTTTTE4R45YVTTTTTTESLPVSNKSJ7537HLRSWP36FP2QYVQCI4WZ727CT)

A dialog is a component of web interfaces providing important information and requiring user interaction in response. The user’s attention should be focused exclusively on the active dialog element, both visually and interactively. Information and user actions presented by the dialog should be phrased simply and unambiguously. **So, a dialog is interruptive by nature and should be used sparingly.**

## Usage

Imagine a user browsing a web application from a mobile phone. The application needs the user’s decision about its settings immediately in order to keep functioning properly – like enabling location services in order to give directions on a map. This could be a use case for a dialog:

1.  The dialog pops up. Only the dialog is interactive, lying over the rest of the content.
2.  The dialog’s header explains the required actions in short. The dialog’s body may contain more detailed information.
3.  One or more interactive elements provide possible user actions in order to find a solution.

## Structure

A modal dialog consists of a container, title, description, buttons and a backdrop. If the user’s flow browsing the application must be interrupted anyway, the least we can do is to present the user a concise and well structured, clear dialog to attract their focus and quickly make an action in order to continue browsing.

![](https://i7x7p5b7.stackpathcdn.com/codrops/wp-content/uploads/2021/10/dialog-structure.jpg)

Basic structure of a modal dialog

It’s essential to phrase a clear and unambiguous message in the title, so the reader can understand it at a glance. Here is one example:

-   Not such a good title: “Do you want to proceed?”
-   Better, because direct and clear: “Allow access to the file system?”

Of course, further information can be put in the body of the dialog itself, but the gist should be comprehensible by reading the title and button texts only.

## Behavior

A dialog always needs to suit a notable purpose: Getting the user to make a choice in order to finish a task or to keep the application functioning properly (like enabling location services for navigation).

### Should the dialog close by clicking the backdrop?

Well, I only asked myself this question _after_ trying to implement that behavior with the native dialog element. As it turns out, it’s far easier with ordinary divs to achieve.

Without the native dialog element, your markup would look something like this:

    <div id="dialog" role="dialog" aria-modal="true">
      
    </div>
    <div class="backdrop"></div>

And the corresponding CSS

    .backdrop {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
    }

Here we have an ordinary div stretching out over the full viewport. We can easily grab that `div.backdrop` with JavaScript and implement our “close-modal-on-click-behavior”.

    const backdrop = document.querySelector(".backdrop");
    const dialog = document.getElementById("dialog");

    backdrop.addEventListener("click", function() { dialog.style.display = none; });

#### So, _why_ can’t we do exactly this with the native dialog element?

The native dialog element comes with a _pseudo-element_ called `::backdrop` when invoked with `dialog.showModal()`. As the name suggests, it is not part of the DOM, and so we cannot access it using JavaScript…

How can we add an event listener on an element, which is essentially not part of the DOM? Well, there are workarounds, like detecting a click outside of the active dialog, but that’s a completely different story.

And once I’ve come to understand, that it is not that easy, I’ve revisitied the initially posed question: Is it worthwhile to close the dialog on click outside?

No, it is not. Keep in mind, that we wanted the user to make a decision. We interrupted the user’s flow of browsing the application. We phrased the message clearly and directly, so that it’ll be comprehensible at a glance. And then we allow the user to dismiss everything we have carefully put together _with a single click?!_ I don’t think so.

## Implementation

When we implement a dialog, the following requirements must be observed carefully:

-   Focus the first interactive element inside the modal once the dialog opens (\*)
-   Trap focus inside the modal dialog
-   Provide at least one button that closes the dialog
-   Prevent interaction with the rest of the page (\*)
-   Close the dialog when the user presses ESC (\*)
-   Return focus to the element, that opened the dialog in the first place

Requirements marked with (\*) are handled by the native dialog element out of the box, when opened as a modal.

So in order to get all the benefits listed above, we’re going to invoke the dialog using the method `showModal` provided to us by the native dialog JavaScript API.

     const dialog = querySelector("dialog");
    dialog.showModal(); 

### Example HTML structure

    <button id="open_dialog">Open Dialog</button>

    <dialog
      aria-labelledby="dialog_title"
      aria-describedby="dialog_description"
    >
      <img
        src="./location-service.svg"
        alt="Illustration of Location Services"
      />
      <h2 id="dialog_title" class="h2">Use location services?</h2>
      <p id="dialog_description">
        In order to give directional instructions, we kindly ask you to turn
        on the location services.
      </p>
      <div class="flex flex-space-between">
        <button id="close_dialog">Close</button>
        <button id="confirm_dialog" class="cta">Confirm</button>
      </div>
    </dialog>

Because we’re using the native dialog element here, we do not need to use `role="dialog"`, `modal="true"` or similar for an accessible implementation.

Based on this simple HTML structure, which is taken from the example CodePen shown at the end of this article, we can now go ahead and implement the requirements listed above. Once the reader clicks the “Open Dialog” button, the first interactive element inside the dialog will receive focus by default.

### Return focus to last active element after closing the dialog

The HTML of a modal dialog can be placed nearly anywhere in the page’s markup. So, when the reader opens the modal, the user agent jumps to the dialog’s markup, like using a portal. Once the reader closes the dialog again, the focus needs to be returned back to the element that the reader was interacting with before opening the dialog. The portal to and from the dialog should go two-way, otherwise the reader will get lost.

    const dialog = document.querySelector("dialog");
    const openDialogBtn = document.getElementById("open_dialog");
    const closeDialogBtn = document.getElementById("close_dialog");

    const openDialog = () => {
      dialog.showModal();
    };

    const closeDialog = () => {
      dialog.close();

      
      
      openDialogBtn.focus();
    };

    openDialogBtn.addEventListener("click", openDialog);
    closeDialogBtn.addEventListener("click", closeDialog);

    const closeDialog = (event) => {
      event.preventDefault();
      dialog.close();
      openDialogBtn.focus();
    };

### Trap focus inside the dialog while open

A focus trap is often a horror regarding UX – in case of a modal dialog it serves an essential purpose: Keeping the reader’s focus on the dialog, helping to prevent interaction with the background.

Based on the same markup and existing JS above, we can add the focus trap to our script.

    const trapFocus = (e) => {
      if (e.key === "Tab") {
        const tabForwards = !e.shiftKey && document.activeElement === lastElement;
        const tabBackwards = e.shiftKey && document.activeElement === firstElement;

        if (tabForwards) {
          
          
          e.preventDefault();
          firstElement.focus();
        } else if (tabBackwards) {
          
          e.preventDefault();
          lastElement.focus();
        }
      }
    };

    const openDialog = () => {
      dialog.showModal();
      dialog.addEventListener("keydown", trapFocus);
    };

    const closeDialog = () => {
      dialog.removeEventListener("keydown", trapFocus);
      dialog.close();
      openDialogBtn.focus();
    };

### Disable closing the dialog on ESC

Just in case you want to disable the built-in functionality of closing the dialog once the user has pressed the ESC key, you can listen for the keydown event when the dialog opens and prevent its default behavior. Please remember to remove the event listener after the modal has closed.

Here is the code to make it happen:

     const dialog = document.querySelector("dialog");

    const openDialog = () => {
      
      dialog.addEventListener("keydown", (e) => {
        if (e.key === "Escape") {
          e.preventDefault();
        }
      });
    };

### Styles for the dialog element

The user agents provide some default styles for the dialog element. To override these and apply our own styles, we can use this tiny CSS reset.

    dialog {
      padding: 0;
      border: none !important;
      
    }

Admittedly, there are more default user agent styles, which center the dialog inside the viewport and prevent overflowing content. We’ll leave these default styles untouched, because in our case they are desirable.

#### CSS ::`backdrop` pseudo-element

Perhaps the coolest thing about the native dialog element is, that it gives us a nice `::backdrop` pseudo-element right out of the box. The serves several purposes for us:

-   Overlay to prevent interaction with the background
-   Easily style the surroundings of the dialog while open

### Accessibility aspects of a dialog element

To ensure accessibility of your modal dialog you’ve already got a great deal covered by simply using the native HTML dialog element as a modal – i.e. invoked with `dialog.showModal()`. Thus, the first interactive element will receive focus, once the dialog opens. Additionally, interaction with other content on the page will be blocked while the dialog is active. Plus, the dialog closes with a keystroke on `ESC`. Everything coming “for free” along with the native dialog element.

In contrast to using a generic `div` as a wrapper instead of the semantically correct dialog element, you do not have to put `role="dialog"` accompanied by `aria-modal="true`.

Apart from these benefits the native dialog element has to offer, we need to make sure the following aspects are implemented:

-   Put a label on the dialog element – e.g. `<dialog aria-label="Use location services?">` or use `aria-labelledby` if you want to reference the ID of another element inside the dialog’s body, which presents the title anyway
-   If the dialog message requires additional information, which may already be visible in the dialog’s body, you can optionally reference this text with `aria-describedby` or phrase a description just for screen readers inside an `aria-description`
-   Return focus to the element, which opened the dialog in the first place, if the dialog has been triggered by a click interaction. This is to ensure that the user can continue browsing the site or application from the same point of regard where they left off before the dialog popped up.

### Polyfill for the native dialog element

Sadly, the native HTML dialog element still lacks browser support here and there. As of this writing, Chrome, Edge and Opera support it, Firefox hides support behind a flag. No support from Safari and IE. The support coverage is around 75% globally. [Reference browser support](https://caniuse.com/dialog)

On the bright side, the dialog element is easily polyfilled with this [dialog polyfill from GoogleChrome](https://github.com/GoogleChrome/dialog-polyfill).

In order to load the polyfill only on those browsers not supporting the dialog element, we check if `dialog.showModal` is not a function.

    const dialog = document.querySelector("dialog");

    if (typeof dialog.showModal !== "function") {
      
      const polyfill = document.createElement("script");
      polyfill.type = "text/javascript";
      polyfill.src = "dist/dialog-polyfill.js"; 
      document.body.append(polyfill);

      
      polyfill.onload = () => {
        dialogPolyfill.registerDialog(dialog);
      };

      
      const polyfillStyles = document.createElement("link");

      polyfillStyles.rel = "stylesheet";
      polyfillStyles.href = "dialog-polyfill.css";
      document.head.append(polyfillStyles);
    }

### Example of a styling a native dialog element

Here is a CodePen showing off an accessible, polyfilled modal dialog. It implements the requirements listed above regarding accessibility, managing focus and polyfill on-demand. The style is based on [Giovanni Piemontese’s](https://dribbble.com/GioPiemontese) [Auto Layout Dialogs – Figma UI Kit](https://dribbble.com/shots/14764086-Auto-Layout-Dialogs-Figma-UI-Kit).

Apart from CodePen, you can view the source code of the example [here on GitHub](https://github.com/christiankozalla/native-dialog-demo). A live example of that [native dialog is hosted here](https://native-dialog.surge.sh/).

## Wrapping up

In this tutorial discussed the structure and purpose of dialogs regarding user-experience, especially for modal dialogs. We’ve compiled a list of requirements for creating user-friendly dialogs. Naturally, we’ve gone in-depth on the native dialog HTML element and the benefits we gain from using it. We’ve extended its functionality by building a focus trap and managing focus around the life-cycle of the native dialog altogether.

We’ve seen how to implement an accessible modal dialog based on the requirements we set before. Our implementation will be polyfilled only when necessary.

Finally, I’ve noticed during my research about the native dialog element, that its reputation in the community has changed alot over the years. It may have been welcomed with an open mind, but today’s opinions are predominantly criticizing the dialog element while simultaneously advising to rather rely on libraries.

Nevertheless, I’m sure the native dialog element has proven to be a suitable basis for implementing modal dialogs in this tutorial. I definitely had some fun!

Thanks for reading, I hope you enjoyed it!

## Related

Another tutorial that might be interesting to you is this one by Osvaldas Valutis, where you’ll learn [how to style and customize the upload button](https://tympanus.net/codrops/2015/09/15/styling-customizing-file-inputs-smart-way/) (file inputs). 
 [https://tympanus.net/codrops/2021/10/06/how-to-implement-and-style-the-dialog-element/](https://tympanus.net/codrops/2021/10/06/how-to-implement-and-style-the-dialog-element/) 
 [https://tympanus.net/codrops/2021/10/06/how-to-implement-and-style-the-dialog-element/](https://tympanus.net/codrops/2021/10/06/how-to-implement-and-style-the-dialog-element/)
