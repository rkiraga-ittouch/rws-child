
# Related website sets demo

Komenda do uruchomienia przeglądarki - odpowienie flagi i zbiór powiązanych domen:

`google-chrome --enable-features="FirstPartySets:FirstPartySetsClearSiteDataOnChangedSets/1,StorageAccessAPI,StorageAccessAPIForOriginExtension,PageInfoCookiesSubpage,PrivacySandboxFirstPartySetsUI" \  
--use-first-party-set="{\"primary\": \"https://rkiraga-itt.github.io/rws-main\", \"associatedSites\": [\"https://rkiraga-ittouch.github.io/rws-child\"]}" \  
https://rkiraga-ittouch.github.io/rws-child`

Demo składa się z dwóch prostych stron zahostowanych na githubpages, pokazują działanie dwóch metod nowego API `Storage Access API`:

* https://github.com/rkiraga-itt/rws-main/blob/main/index.html
* https://github.com/rkiraga-ittouch/rws-child/blob/main/index.html

## **1. Case 1 - zagnieżdzony iframe który jest w innej domenie.**

https://rkiraga-ittouch.github.io/rws-child/

[Metoda Document.requestStorageAccess() (rSA)](https://developer.chrome.com/docs/privacy-sandbox/related-website-sets-integration/#checking-and-requesting-storage-access)

W standardowym przypadku, przy wyłączonych 3rd party cookies, iframe nie uzyskałby dostępu do storage przeglądarki. 

Api SAA pozwala na sprawdzenie czy obecnie taki dostęp jest udzielony oraz na wysłanie żądania do jego uzyskania.
Demo pokazuje, że po uzyskaniu zgody (zapytanie + ewentualnie wymagany user-gesture - np. kliknięcie) może odczytać plik cookie.

### **Demo:**

Należy wejść najpierw na stronę https://rkiraga-itt.github.io/rws-main/ i kliknąć pierwszy button w celu utworzenia ciasteczka.

Następnie na stronie: https://rkiraga-ittouch.github.io/rws-child/
odpalamy narzędzia deweloperskie. Możemy zaobserwować, że strony child oraz iframe nie mają dostępu do ciasteczka z domeny main.

Klikamy button "Click me to try to get cookies..."
Jeśli to wymagane, pojawi się drugi przycisk "permission trigger button", który również należy kliknąć.

Rezultatem jest udzielenie dostępu do storage przeglądarki. W consolu logu można zaobserwować, że rzeczywiście możemy
odczytać ciasteczko "mainCookie".

## **2. Case 2 - strona próbuje pobrać resource wymagający cookiesa z innej domeny.**

https://rkiraga-ittouch.github.io/rws-child/index2.html

[Document.requestStorageAccessFor() (rSAFor).](https://developer.chrome.com/docs/privacy-sandbox/related-website-sets-integration/#requeststorageaccessfor-in-chrome)

SAA only allows embedded sites to request access to storage from within `iframe` elements that have received user interaction.  
This poses challenges in adopting SAA for top-level sites that use cross-site images or script tags requiring cookies.

### **Demo:**
Strona przy uruchomieniu próbuje pobrać resource z innej domeny. W devtoolsach widać, że cookie nie jest dołączany do requestu.

Przycisk pozwala na wysłanie żądania do przeglądarki o uzyskanie dostępu do storage. 
Jeśli to wymagane, dodatkowo tworzony jest kolejny przycisk, który pozwala na interkacje użytkownika, należy go kliknąć.

Po uzyskaniu dostępu do storage, przy kolejnym wysłaniu requestu do innej domeny, cookie jest dołączany.

