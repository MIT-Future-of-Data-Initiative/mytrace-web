<script>
  
  export let issues = [];
  import TaxonomyLink from "$lib/TaxonomyLink.svelte";
</script>
<div class="trace-agent" style="border-left-color: {issues.length > 0 ? 'red' : '#3399ff'}">
  <div class="trace-header">
    <span class="icon">🔍</span>
    <strong>TraceAgent</strong>
  </div>
  {#if issues.length === 0}
    <p class="trace-message">No problems detected</p>
  {:else}
    {#each issues as issue}
      <div>
        {#if issue.highlight.includes("TPDSA")}
          <p class="trace-message">
            You never received a privacy notice, consented, nor were given a way to opt out of <b>{issue.expected}</b> processing your sensitive data. This is in violation of <b>TPDSA 541.101(b)(4)</b> (consent), <b>TPDSA 541.102(a)(1) and 541.102(b)</b> (privacy notice), and <b>TPDSA 541.103</b> (opt-out).
          </p>
        {:else if issue.highlight.includes("Tex Ins Code")}
          <p class="trace-message">
            <b>Allstate</b> used sensitive data that <b>Arity</b> collected illegally. This is in violation of the <b>Texas Insurance Code 541.001</b>.
          </p>
        {:else}
          <p class="trace-message">
            Your data was used for <b><TaxonomyLink term={issue.found} /></b><br />
            when you only allowed use for <b><TaxonomyLink term={issue.expected} /></b>.
          </p>
        {/if}
        <button class="report-button">
          Report This
        </button>
      </div>
    {/each}
  {/if}
</div>

<style>
.trace-agent {
  width: 160px;
  flex-shrink: 0;
  padding: 1rem;
  background-color: white;
  border-left: 4px solid;
  display: flex;
  flex-direction: column;
  font-size: 0.875rem;
  border-radius: 0;
  margin-bottom: 1rem;
}

.trace-header {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 0.25rem;
}
.trace-message {
  margin: 0;
}
</style>
