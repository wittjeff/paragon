---
title: 'Dropzone'
type: 'component'
components:
- Dropzone
categories:
- Forms
status: 'New'
designStatus: 'Done'
devStatus: 'Done'
notes: 
---

The `Dropzone` allows users to upload files via drag and drop, or by clicking the component. Currently, only one file upload at a time is allowed.

You will also need to provide upload logic yourself via `onProcessUpload` prop which accepts function that should take care of uploading the file to the backend (i.e. send HTTP request). 
This function accepts an object with following content as its only argument:
- \{object\} fileData - Metadata about the uploaded file.
- \{object\} requestConfig - Config to pass to `axios` call (this is required to display progress bar and hande cancel action).
- \{function\} handleError - Function to communicate to `Dropzone` that file upload resulted in failure, expects `Error` object as its only argument.

Each example below implements such a function.

**Note** that `Dropzone` does not render file after successful upload, you will have to provide that logic yourself depending on which type of file has been uploaded, see [this example](#with-file-preview).

## Basic Usage
Drag and drop a file to begin uploading, only one file at a time is allowed, no other validation is done.

- Use `onUploadProgress` prop to get feedback about the upload progress - should be a function that receives (percentageUploaded, progressEvent) as arguments.

```jsx live
() => {
  async function handleProcessUpload({
    fileData, requestConfig, handleError
  }) {
    const uploadUrl = 'https://httpbin.org/post';
    try {
      const response = await axios.post(uploadUrl, fileData, requestConfig);
    } catch (error) {
      handleError(error);
    }
  };

  return (
    <Dropzone
      onProcessUpload={handleProcessUpload}
      onUploadProgress={(percent) => console.log(percent)}
    />
  )
}
```

## With Progress Bar
Display upload progress as a progress bar with the ability to cancel the upload.

- Use `onUploadCancel` prop to react to upload cancel event - should be a function that takes `Response` object as its argument. 

```jsx live
() => {
  async function handleProcessUpload({
    fileData, requestConfig, handleError
  }) {
    const uploadUrl = 'https://httpbin.org/post';
    try {
      const response = await axios.post(uploadUrl, fileData, requestConfig);
    } catch (error) {
      handleError(error);
    }
  };

  return (
    <Dropzone
      onProcessUpload={handleProcessUpload}
      onUploadCancel={() => console.log('UPLOAD CANCEL')}
      progressVariant="bar"
    />
  )
}
```

## With file size and type validation

Accepts only .png files with size between 1MB and 20MB. The file sizes are specified in bytes.
`accept` prop should be an object with the keys set to the [MIME type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Common_types) and the values to an array of file extensions.

For example:
- to allow only PNG images (as in this example) you should pass `{ 'image/*': ['.png'] }` object as `accept` prop;
- to allow both PNG and JPG images you should pass `{ 'image/*': ['.png', '.jpg'] }` object as `accept` prop;
- to allow arbitrary images you should pass `{ 'image/*': [] }` object as `accept` prop

The component will render a helpful message about size and type restrictions based on the values you pass to `accept`, `minSize` and `maxSize` props.

```jsx live
() => {
  async function handleProcessUpload({
    fileData, requestConfig, handleError
  }) {
    const uploadUrl = 'https://httpbin.org/post';
    try {
      const response = await axios.post(uploadUrl, fileData, requestConfig);
    } catch (error) {
      handleError(error);
    }
  };

  return (
    <Dropzone
      onProcessUpload={handleProcessUpload}
      onUploadProgress={(percent) => console.log(percent)}
      onUploadCancel={() => console.log('UPLOAD CANCEL')}
      progressVariant="bar"
      minSize={1048576}
      maxSize={20 * 1048576}
      accept={{
        "image/*": ['.png'],
      }}
    />
  )
}
```

## With file preview

Shows image after successful upload.

```jsx live
() => {
  const [uploadedFile, setUploadedFile] = useState(undefined);

  async function handleProcessUpload({
    fileData, requestConfig, handleError
  }) {
    const uploadUrl = 'https://httpbin.org/post';
    try {
      const response = await axios.post(uploadUrl, fileData, requestConfig);
      setUploadedFile(response.data.files.file);
    } catch (error) {
      handleError(error);
    }
  };

  if (uploadedFile) {
    return <Image src={uploadedFile} fluid alt="Image description" />;
  }

  return (
    <Dropzone
      onProcessUpload={handleProcessUpload}
      onUploadProgress={(percent) => console.log(percent)}
      onUploadCancel={() => console.log('UPLOAD CANCEL')}
      progressVariant="bar"
      accept={{
        "image/*": [],
      }}
    />
  )
}
```

## With custom error messages

Accepts only .png files with size between 1MB and 20MB, renders custom validation error messages.

```jsx live
() => {
  async function handleProcessUpload({
    fileData, requestConfig, handleError
  }) {
    const uploadUrl = 'https://httpbin.org/post';
    try {
      const response = await axios.post(uploadUrl, fileData, requestConfig);
    } catch (error) {
      handleError(error);
    }
  };

  return (
    <Dropzone
      onProcessUpload={handleProcessUpload}
      onUploadProgress={(percent) => console.log(percent)}
      onUploadCancel={() => console.log('UPLOAD CANCEL')}
      errorMessages={{
        invalidType: 'Invalid file type, only images allowed.',
        invalidSize: 'The file size must be between 1MB and 20MB.',
        multipleDragged: 'Cannot upload more than one file.',
      }}
      progressVariant="bar"
      minSize={1048576}
      maxSize={20 * 1048576}
      accept={{
        "image/*": ['.png'],
      }}
    />
  )
}
```

## With custom look

Use `inputComponent` prop to override default view of `Dropzone`.

```jsx live
() => {
  const MyInputComponent = <p>Hey! You can render here anything you want ;)</p>;

  async function handleProcessUpload({
    fileData, requestConfig, handleError
  }) {
    const uploadUrl = 'https://httpbin.org/post';
    try {
      const response = await axios.post(uploadUrl, fileData, requestConfig);
    } catch (error) {
      handleError(error);
    }
  };

  return (
    <Dropzone
      onProcessUpload={handleProcessUpload}
      onUploadProgress={(percent) => console.log(percent)}
      inputComponent={MyInputComponent}
    />
  )
}
```

## With custom validation

You can pass your own validator function which accepts `File` object as its only argument, the function is invoked after all other validation steps are done.
The function should return error message to display in case validation fails, otherwise return `null`.

Note that `Dropzone` does not handle unexpected errors that might happen in your function, they should be handled by the validator itself as in the example below.

This example validates that only `400x479` images can be uploaded. 

```jsx live
() => {
  // note that we do not need to validate that received file is actually an image
  // because 'accept' parameter handles that before validation function is used
  async function imageDimensionValidator(file) {
    const image = new window.Image();
    try {
      const url = URL.createObjectURL(file);
      image.src = url;
      await image.decode();
      if (image.width !== 400 || image.height !== 479) {
        return 'The image must have 400x479 dimensions.';
      }
    } catch (error) {
      return 'Unexpected error happened during file validation, please try again.'
    }
    return null;
  }

  async function handleProcessUpload({
    fileData, requestConfig, handleError
  }) {
    const uploadUrl = 'https://httpbin.org/post';
    try {
      const response = await axios.post(uploadUrl, fileData, requestConfig);
    } catch (error) {
      handleError(error);
    }
  };

  return (
    <Dropzone
      onProcessUpload={handleProcessUpload}
      onUploadProgress={(percent) => console.log(percent)}
      onUploadCancel={() => console.log('UPLOAD CANCEL')}
      progressVariant="bar"
      accept={{
        "image/*": []
      }}
      validator={imageDimensionValidator}
    />
  );
}
```

## Reading file contents into memory

Accepts only .xml files up to a size of 20MB. You can read in the contents of the `File` object into memory. The ``onProcessUpload`` prop can retrieve the file Blob from the passed ``fileData`` param and either pass it into a file reader or use text() promise.

Note that `Dropzone` does not handle unexpected errors that might happen in your function, they should be handled by the ``handleProcessUpload`` method.

```jsx live
() => {
  const [text, setText] = useState("");
  const [fileName, setFileName] = useState("");

  const handleProcessUpload = ({ fileData }) => {
    const blob = fileData.get('file');
    blob.text().then(xmlText => {
      setText(xmlText);
      setFileName(blob.name);
    });
  };

  return (
    <>
      <Dropzone
        onProcessUpload={handleProcessUpload}
        onUploadProgress={(percent) => console.log(percent)}
        onUploadCancel={() => console.log('UPLOAD CANCEL')}
        progressVariant="bar"
        maxSize={20 * 1048576}
        accept={{
          "application/xml": ['.xml']
        }}
      />
      {fileName && (
        <Form.Control.Feedback type="valid">
          Uploaded{' '}
          {fileName}
        </Form.Control.Feedback>
      )}
      <p>{text}</p>
    </>
  )
}
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`Dropzone` is built on [react-dropzone](https://react-dropzone.js.org/) (v14). There is no APG
pattern for a drag-and-drop file upload; the accessible path is the keyboard/click flow, which
opens the native file picker. Several structural gaps are tracked in
[#3957](https://github.com/openedx/paragon/issues/3957) — see Known issues.

### Keyboard support

Provided by react-dropzone:

| Key | Action |
| --- | --- |
| <kbd>Tab</kbd> | Moves focus to the dropzone container (`tabindex="0"`). |
| <kbd>Enter</kbd> / <kbd>Space</kbd> | Opens the native file picker dialog. |

While an upload is in progress the dropzone is disabled; with the `bar` variant a "Cancel" button
(a native `<button>` with visible text) becomes the actionable element.

### ARIA roles, states, and properties

What the shipped code renders (confirmed by unit test snapshots):

| Element | Attribute | Notes |
| --- | --- | --- |
| Container (`<div>`) | `role="presentation"`, `tabindex="0"` | Provided by react-dropzone. A focusable, keyboard-operable element removed from the accessibility tree and given no accessible name — see Known issues. |
| File input (`<input type="file">`) | `tabindex="-1"`, visually hidden | No label, `aria-label`, or `title` — no accessible name. |
| Instruction text ("Drag and drop your file here or click to upload.") | plain `<p>` | Localized, but not programmatically associated with the container or input. |
| Upload icon | `aria-hidden="true"`, `focusable="false"` | Decorative. |
| Validation errors (`GenericError`) | `role="alert"` | Provided by React-Bootstrap `Alert`; error text is announced when rendered. |
| Upload progress (`spinner` variant) | `role="status"`, `aria-live="polite"` | Announces "Uploading &lt;name&gt;, &lt;percent&gt;% done." via the `Spinner`'s visually hidden text. |
| Upload progress (`bar` variant) | `role="progressbar"`, `aria-valuenow` / `aria-valuemin` / `aria-valuemax` | Provided by React-Bootstrap `ProgressBar`. |
| Multiple-files drag error (`DragError`) | plain `<div>` | Shown only during a drag; no live region, so it is not announced. |

### Screen reader expectations

- Because the container is `role="presentation"` and the file input is unnamed, focusing the
  dropzone announces little or nothing useful — likely a bare "clickable" or silence, depending on
  the screen reader. *(Verification pending per the policy's AT matrix; this is the core defect in
  [#3957](https://github.com/openedx/paragon/issues/3957).)*
- Validation errors are announced when they appear (alert). Upload progress with the `spinner`
  variant is announced politely. *(Verification pending per the policy's AT matrix.)*

### Application responsibilities

- Until [#3957](https://github.com/openedx/paragon/issues/3957) is resolved, precede the dropzone
  with visible text (or a heading) describing what to upload, and consider passing `role` and
  `aria-label` props (they are spread onto the container) to replace `role="presentation"` with
  `role="button"` plus a name.
- If you replace the default content via `inputComponent`, keep an equivalent of the instruction
  text — and remember drag-and-drop is unavailable to keyboard and most screen reader users, so
  never describe the interaction as drag-only.
- Supply localized `errorMessages`; the defaults are localized, but custom strings are rendered
  verbatim.
- After a successful upload, render a visible, focusable confirmation (this component intentionally
  renders nothing on success).

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Form field has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/e086e5/) — the hidden file input
- [Element marked as decorative is not exposed](https://www.w3.org/WAI/standards-guidelines/act/rules/46ca7f/) — `role="presentation"` on a focusable container
- [Button has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/) — the upload-cancel button
- [ARIA state or property has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/6a7281/) — progress bar values
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/)

### Known issues

Tracked in [#3957](https://github.com/openedx/paragon/issues/3957), identified in the August 2026
accessibility audit:

- The focusable, keyboard-operable container has `role="presentation"` and no accessible name
  (inherited from react-dropzone defaults), and the file input it drives is also unnamed — the
  control is effectively anonymous to assistive technology (WCAG 4.1.2).
- The instruction text is not programmatically associated with the control.
- The multiple-files drag error is not announced (no live region) and only exists during a drag.
