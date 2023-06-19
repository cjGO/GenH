<script>
  import { onMount } from 'svelte';
  let data = null;

  onMount(async () => {
      const proxyurl = "https://cors-anywhere.herokuapp.com/";
      const url = "https://api.genhealth.ai/predict";
      const headers = {
          'Content-Type': 'application/json',
          'Authorization': 'Token gh_s_k-YWZiZDY4MzQtOTA4Ni00MTljLTk5NWYtOGE3NThlMjk2ZWNl'
      };
      const body = JSON.stringify({
          'history': [
  {
    "code": "81",
    "system": "age",
    "display": "81"
  },
  {
    "code": "3021F",
    "system": "CPT4",
    "display": "Left ventricular ejection fraction (LVEF) less than 40% or documentation of moderately or severely depressed left ventricular systolic function (CAD, HF)"
  },
  {
    "code": "I50.89",
    "system": "ICD10CM",
    "display": "Other heart failure"
  },
  {
    "code": "R94.4",
    "system": "ICD10CM",
    "display": "Abnormal results of kidney function studies"
  },
  {
    "code": "E87.6",
    "system": "ICD10CM",
    "display": "Hypokalemia"
  },
  {
    "code": "551546933",
    "system": "NDC",
    "display": "dapagliflozin 10mg/1 ORAL TABLET, FILM COATED"
  },
  {
    "code": "00-01-month",
    "system": "timegap",
    "display": "00-01-month"
  },
  {
    "code": "68788788308",
    "system": "NDC",
    "display": "carvedilol 6.25 MG Oral Tablet"
  },
  {
    "code": "00228239096",
    "system": "NDC",
    "display": "Hydrochlorothiazide 25 MG / Spironolactone 25 MG Oral Tablet"
  },
  {
    "code": "03-06-month",
    "system": "timegap",
    "display": "03-06-month"
  },
  {
    "code": "006158394",
    "system": "NDC",
    "display": "bumetanide 2mg/1 ORAL TABLET"
  },
  {
    "code": "03-06-month",
    "system": "timegap",
    "display": "03-06-month"
  },
  {
    "code": "70756081430",
    "system": "NDC",
    "display": "hydrochlorothiazide 25 MG / olmesartan medoxomil 40 MG Oral Tablet"
  },
  {
    "code": "female",
    "system": "gender",
    "display": "female"
  }
],
          'num_predictions': 10,
          'generation_length': 50,
          'inference_threshold': 0.95,
          'inference_temperature': 0.95
      });

      try {
          const response = await fetch(proxyurl + url, {
              method: 'POST',
              headers: headers,
              body: body
          });
          if (response.ok) {  // if HTTP-status is 200-299
              // get the response body
              data = await response.json();
          } else {
              console.error("HTTP-Error: " + response.status);
          }
      } catch (error) {
          console.error(error);
      }
  });
</script>

<main>
  <h1>Data from API</h1>
  {#if data}
      <pre>{JSON.stringify(data, null, 2)}</pre>
  {:else}
      <p>Loading...</p>
  {/if}
</main>
