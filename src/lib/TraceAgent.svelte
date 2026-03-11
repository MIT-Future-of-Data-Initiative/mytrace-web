<script>
  
  export let issues = [];
  import TaxonomyLink from "$lib/TaxonomyLink.svelte";
  import { base } from '$app/paths';
  let showPopup = false;

  let violationIndex = 0;

  const tpdsaViolations = [
    "Violation 1: TPDSA 541.102(a)(1) - You never received a privacy notice before your sensitive data was processed.",
    "Violation 2: TPDSA 541.101(b)(4) - No explicit consent was obtained for processing sensitive data.",
    "Violation 3: TPDSA 541.102(b) - Arity did not include the following notice: NOTICE: We may sell your sensitive personal data.",
    "Violation 4: TPDSA 541.103 - No opt-out given.",
    "Violation 5: TPDSA 541.102(a)(3) and 541.051(b)(5) - No TPDSA rights notice given.",
    "Violation 6: Data Broker Law - Arity is not registered with the Texas Secretary of State’s Office."
  ];
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
          <p class="trace-message clickable" on:click={() => showPopup = true}>
            6 violations of Texas law
          </p>
        {:else if issue.highlight.includes("Tex Ins Code")}
          <p class="trace-message clickable" on:click={() => showPopup = true}>
            1 violation of Texas law
          </p>
        {:else}
          <p class="trace-message">
            Your data was used for <b><TaxonomyLink term={issue.found} /></b><br />
            when you only allowed use for <b><TaxonomyLink term={issue.expected} /></b>.
            
            <button class="report-button">
              Report This
            </button>
          </p>
        {/if}
        {#if showPopup}
            <div class="popup" on:click|stopPropagation>
                  {#if issue.highlight.includes("TPDSA")}
                      <p>{tpdsaViolations[violationIndex]}</p>
                      <!-- You never received a privacy notice, consented, nor were given a way to opt out of <b>{issue.expected}</b> processing your sensitive data. This is in violation of <b>TPDSA 541.101(b)(4)</b> (consent), <b>TPDSA 541.102(a)(1) and 541.102(b)</b> (privacy notice), and <b>TPDSA 541.103</b> (opt-out). -->
                      <button class="report-button">
                        Report This
                      </button>
                      <div class="nav">
                        <button on:click={() => violationIndex = Math.max(0, violationIndex - 1)}>&lt;</button>

                        <span>{violationIndex + 1} / 6</span>

                        <button on:click={() => violationIndex = Math.min(5, violationIndex + 1)}>&gt;</button>
                      </div>
                  {:else if issue.highlight.includes("Tex Ins Code")}
                    <p>
                      <b>Allstate</b> used sensitive data that <b>Arity</b> collected illegally. This is in violation of the <b>Texas Insurance Code 541.001</b>.
                    </p>
                    <button class="report-button">
                      Report This
                    </button>
                  {/if}
              <button class="close-button" on:click={() => showPopup = false}>×</button>
              
            </div>
        {/if}
        
          <a href="{base}/config-ta" class="card-footer">
              Not in Texas? Configure TraceAgent
            </a>
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
.clickable {
  cursor: pointer;
  font-weight: bold;
  color: red;               
  text-decoration: none;
}
.clickable:hover {                
  text-decoration: underline;
}

.popup {
  position: relative;
  background: white;
  border: 1px solid #ccc;
  padding: 1rem;
  margin-top: 0.5rem;
  border-radius: 4px;
}

.close-button {
  position: absolute;
  top: 4px;
  right: 6px;
  border: none;
  background: none;
  font-size: 1rem;
  cursor: pointer;
  color: #666;
}

.close-button:hover {
  color: black;
}

.nav {
  display: flex;
  justify-content: left;
  align-items: center;
  gap: 0.5rem;
  margin-top: 0.7rem;
}

.nav button {
  border: none;
  background: none;
  cursor: pointer;
  font-weight: bold;
  font-size: 1rem;
}
</style>
