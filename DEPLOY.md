# LeadDesk: GitHub Pages deploy guide

Is folder mein sab kuch ready hai: `index.html`, `manifest.json`, `sw.js`, `icons/`, `.nojekyll`.
Deploy hone ke baad app ek HTTPS link pe chalegi, kisi bhi phone pe khulegi, aur "Install app" ho sakegi.

---

## Option A: Browser se (git nahi aata to, 5 minute)

1. github.com pe login karo, **New repository** dabao.
2. Repository name: `leaddesk` (ya jo chaho). **Public** rakho (free Pages ke liye zaroori hai). **Create repository**.
3. Naye repo ke page pe **uploading an existing file** link dabao.
4. Is folder ki **saari files aur `icons` folder** drag-drop karo. `index.html` root mein honi chahiye, kisi subfolder mein nahi.
   - `.nojekyll` file hidden hoti hai. Agar drag-drop mein na aaye to koi dikkat nahi, app uske bina bhi chalegi.
5. **Commit changes** dabao.
6. Repo mein **Settings > Pages** kholo.
7. **Build and deployment > Source: Deploy from a branch**. Branch `main`, folder `/ (root)`. **Save**.
8. 1-2 minute rukho. Page ke upar link aayega:
   `https://<tumhara-username>.github.io/leaddesk/`

---

## Option B: Command line se (laptop pe)

Pehli baar ek hi baar setup:
```bash
git config --global user.name "Dharamveer"
git config --global user.email "tumhara-github-email@example.com"
```

Phir is folder ke andar:
```bash
git init -b main
git add .
git commit -m "LeadDesk v1"
git remote add origin https://github.com/<username>/leaddesk.git
git push -u origin main
```
Push ke waqt password nahi chalega. Personal Access Token chahiye:
GitHub > Settings > Developer settings > Personal access tokens > Fine-grained token, sirf `leaddesk` repo ko **Contents: Read and write** do. Token ko password ki jagah paste karo.

Agar GitHub CLI (`gh`) installed hai, to aur aasaan:
```bash
gh auth login
gh repo create leaddesk --public --source=. --push
gh api -X POST repos/<username>/leaddesk/pages -f "source[branch]=main" -f "source[path]=/"
```

---

## Phone pe install karna

- **Android (Chrome):** link kholo, menu (3 dots) > **Install app** / Add to Home screen.
- **iPhone (Safari):** link kholo, Share > **Add to Home Screen**.

Ek baar khulne ke baad app offline bhi chalegi.

---

## Update kaise karni hai

1. `index.html` mein change karo.
2. `sw.js` mein `const VERSION = 'leaddesk-v1'` ko `'leaddesk-v2'` karo. Ye zaroori hai, warna phones purana version dikhate rahenge.
3. Dobara upload ya push karo:
```bash
git add . && git commit -m "v2" && git push
```
4. 1-2 minute baad app band karke dobara kholo.

---

## Zaroori baatein (padh lo)

- **Data repo mein nahi jaata.** Leads, quotations sab har phone ke browser mein save hote hain. Isliye public repo safe hai. Phir bhi repo mein kabhi apna asli customer data ya backup `.json` commit mat karna.
- **Har phone ka data alag hai.** Ek phone se doosre mein ले jaane ke liye Settings > Download backup, phir doosre phone mein Restore.
- **Backup roz ya har hafte lo.** Browser data clear ho gaya ya app uninstall hui to leads chale jaayenge.
- **Link badalna:** agar app ka link badalta hai (repo ka naam ya username change), to naye link pe purana data nahi dikhega, kyunki browser storage address ke hisaab se hota hai. Pehle backup lo.
- **Private repo:** GitHub Pages ke liye free plan pe repo public hona chahiye. App ka code public hone se dikkat na ho to theek hai.
- **Custom domain (optional):** Settings > Pages > Custom domain mein `lead.bizzsathi.in` jaisa subdomain daal sakte ho. DNS mein us subdomain ka CNAME record `<username>.github.io` pe point karo. HTTPS "Enforce HTTPS" se on karo.

---

## Agar kuch na chale

| Dikkat | Wajah | Fix |
|---|---|---|
| Link pe 404 | Pages on nahi hua ya `index.html` root mein nahi hai | Settings > Pages dekho, files root mein hain ya nahi |
| Sirf README dikh raha hai | `index.html` ka naam galat hai | Naam bilkul `index.html` (chhote akshar) rakho |
| Update nahi dikh raha | Purana service worker cache | `sw.js` mein VERSION badlo, app band karke kholo |
| "Install app" nahi dikh raha | HTTPS nahi ya icons missing | `https://` link se kholo, `icons/` folder upload hua ya nahi dekho |
| Page khaali ya purana | GitHub deploy chal raha hai | Repo ke **Actions** tab mein "pages build" ka green tick dekho |
