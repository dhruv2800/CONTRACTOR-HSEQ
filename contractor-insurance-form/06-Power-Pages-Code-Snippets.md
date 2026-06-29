# 06 — Power Pages snippets (optional custom code)

You can build the whole portal with the no-code form designer (file `05`). These snippets
are only if you want extra polish.

---

## A — Show the CoR questions only for Transport Operators

On the **Register / Renew** page → **Edit code (</>)** → add to the page's JavaScript.
Adjust the field logical names to match your Dataverse columns.

```javascript
$(document).ready(function () {
  function toggleCoR() {
    var isTransport = $("#rp_transportoperator").val() === "Yes" ||
                      $("#rp_transportoperator option:selected").text() === "Yes";
    $("[data-name='cor_section']").toggle(isTransport);
  }
  $("#rp_transportoperator").on("change", toggleCoR);
  toggleCoR();
});
```

---

## B — Warn if an expiry date is in the past (client-side)

```javascript
$("#rp_expirydate").on("change", function () {
  var d = new Date($(this).val());
  if (d < new Date()) {
    alert("That expiry date is in the past — please upload a current Certificate of Currency.");
  }
});
```

---

## C — Custom "Thank you" web template (Liquid)

Power Pages → **+ Add page → blank → Edit code**, paste:

```liquid
<div class="container text-center" style="padding:60px 0;">
  <h1>✅ Thank you</h1>
  <p>Your contractor details and insurance certificates have been received.</p>
  <p>We'll email you before any of your insurances expire so you stay approved to work
     on our Recycling Parks sites.</p>
  <a class="btn btn-primary" href="/">Back to home</a>
</div>
```

---

## D — File upload notes

The Dataverse **File** column on the `Insurance` table renders as an upload control with
no code. Default max size is 32 MB (configurable on the column). Accepts PDF/JPG/PNG — fine
for Certificates of Currency. Uploaded files are stored in Dataverse and visible to your
team in the model-driven app / table view.
