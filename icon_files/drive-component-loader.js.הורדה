function loadScriptsAndStyles() {
  const baseUrl = window['cua-drive']?.baseUrl;
  const regex = /^(https:\/\/)(?<subdomain>(www|stage))\.tesla\.(?<domain>(com|cn))$/;
  const match = regex.exec(baseUrl);

  if (!match) {
    throw new Error('[Drive component] invalid base url set');
  }
  const isProd = match.groups.subdomain === 'www'; // "www" or "stage"
  const domain = match.groups.domain; // "com" or "cn"
  const baseUrlDrive = `https://cua-test-drive-ui${isProd ? '' : '-stage'}.tesla.${domain}`;

  fetch(`${baseUrlDrive}/asset-manifest.json`)
    .then((res) => {
      if (!res.ok) {
        throw new Error('[Drive component] error fetching asset-manifest');
      }
      return res.json();
    })
    .then((res) => {
      if (res.files) {
        const script = document.createElement('script');
        script.src = `${baseUrlDrive}${res.files['main.js']}`;
        script.async = true;

        const style = document.createElement('link');
        style.href = `${baseUrlDrive}${res.files['main.css']}`;
        style.rel = 'stylesheet';

        document.body.appendChild(script);
        document.body.appendChild(style);
      }
    })
    .catch(() => {
      throw new Error('[Drive component] error fetching asset-manifest');
    });
}

if (document.readyState === 'loading') {
  document.addEventListener('DOMContentLoaded', loadScriptsAndStyles);
} else {
  loadScriptsAndStyles();
}
