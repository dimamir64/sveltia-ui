<script>
  import { Button, Toast } from '$lib';
  import Alert from '$lib/components/alert/alert.svelte';
  import Example from '../../_components/example.svelte';

  let showStandardToast = false;
  let showErrorToast = false;
  let showSuccessToast = false;
  let showWarningToast = false;
  let showPositionToast = false;
  /** @type {ToastPosition | undefined} */
  let showPositionToastPosition = undefined;
  let showManualToast = false;
</script>

<h2>Toast</h2>

<section>
  <h3>Showing an Alert</h3>
  <Example>
    <div role="none">
      <Button
        variant="secondary"
        label="Show Error Toast"
        on:click={() => {
          showErrorToast = true;
        }}
      />
      <Toast bind:show={showErrorToast}>
        <Alert status="error">
          There was an error while saving the entry. Please try again later.
        </Alert>
      </Toast>
    </div>
    <div role="none">
      <Button
        variant="secondary"
        label="Show Warning Toast"
        on:click={() => {
          showWarningToast = true;
        }}
      />
      <Toast bind:show={showWarningToast}>
        <Alert status="warning">Be careful. These settings are only for advanced users.</Alert>
      </Toast>
    </div>
    <div role="none">
      <Button
        variant="secondary"
        label="Show Information Toast"
        on:click={() => {
          showStandardToast = true;
        }}
      />
      <Toast bind:show={showStandardToast}>
        <Alert status="info">Form data will be automatically saved.</Alert>
      </Toast>
    </div>
    <div role="none">
      <Button
        variant="secondary"
        label="Show Success Toast"
        on:click={() => {
          showSuccessToast = true;
        }}
      />
      <Toast bind:show={showSuccessToast}>
        <Alert status="success">Entry has been saved.</Alert>
      </Toast>
    </div>
  </Example>
</section>

<section>
  <h3>Position</h3>
  <Example>
    <div role="none">
      <Button
        variant="secondary"
        label="Top Left"
        on:click={() => {
          showPositionToast = true;
          showPositionToastPosition = 'top-left';
        }}
      />
      <Button
        variant="secondary"
        label="Top Center"
        on:click={() => {
          showPositionToast = true;
          showPositionToastPosition = 'top-center';
        }}
      />
      <Button
        variant="secondary"
        label="Top Right"
        on:click={() => {
          showPositionToast = true;
          showPositionToastPosition = 'top-right';
        }}
      />
      <Button
        variant="secondary"
        label="Bottom Left"
        on:click={() => {
          showPositionToast = true;
          showPositionToastPosition = 'bottom-left';
        }}
      />
      <Button
        variant="secondary"
        label="Bottom Center"
        on:click={() => {
          showPositionToast = true;
          showPositionToastPosition = 'bottom-center';
        }}
      />
      <Button
        variant="secondary"
        label="Bottom Right"
        on:click={() => {
          showPositionToast = true;
          showPositionToastPosition = 'bottom-right';
        }}
      />
      <Toast bind:show={showPositionToast} position={showPositionToastPosition}>
        <Alert status="success">You’ve got a position demo.</Alert>
      </Toast>
    </div>
  </Example>
</section>

<section>
  <h3>Actionable</h3>
</section>

<section>
  <h3>Manual Close</h3>
  <Example>
    <div role="none">
      <Button
        variant="secondary"
        label="Show Toast"
        on:click={() => {
          showManualToast = true;
        }}
      />
      <Button
        variant="secondary"
        label="Hide Toast"
        on:click={() => {
          showManualToast = false;
        }}
      />
      <Toast bind:show={showManualToast} duration={0}>
        <Alert status="success">You need to hide the toast manually.</Alert>
      </Toast>
    </div>
  </Example>
</section>
