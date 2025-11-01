# boykot-bilinci
<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Boykot Bilinci: Mazlumlara Umut Ol</title>
<style>
body {
    margin: 0;
    font-family: Arial, sans-serif;
    background-color: #6B0000; /* Başlığa uyumlu koyu kırmızı */
    color: white;
    text-align: center;
}
header {
    background-color: rgba(0,0,0,0.7);
    padding: 20px;
    font-size: 28px;
    font-weight: bold;
}
input {
    padding: 10px;
    width: 250px;
    border: none;
    border-radius: 8px;
    margin-top: 20px;
}
button {
    padding: 10px 15px;
    border: none;
    background-color: #4B0000;
    color: white;
    border-radius: 8px;
    cursor: pointer;
}
button:hover {
    background-color: #8B0000;
}
.result {
    margin-top: 30px;
    font-size: 20px;
    font-weight: bold;
}
.language-selector {
    margin: 20px;
}
</style>
</head>
<body>

<header id="headerText">Boykot Bilinci: Mazlumlara Umut Ol</header>

<div class="language-selector">
    <label for="language">Dil Seçin: </label>
    <select id="language" onchange="changeLanguage()">
        <option value="tr">Türkçe</option>
        <option value="en">English</option>
        <option value="ar">العربية</option>
    </select>
</div>

<p id="descriptionText">Bir markayı yaz ve boykot listesinde olup olmadığını öğren:</p>
<input type="text" id="brandInput" placeholder="Marka adını yaz...">
<button onclick="checkBrand()">Kontrol Et</button>
<div class="result" id="result"></div>

<script>
const boykotMarkalar = [
"cocacola","pepsi","nestle","starbucks","mcdonalds","burgerking","kfc",
"dominospizza","pizzahut","unilever","proctergamble","danone","loreal",
"zara","hm","nike","adidas","puma","amazon","disney","netflix","shell",
"hp","hewlettpackard","intel","google","facebook","meta","paypal",
"johnsonandjohnson","colgatepalmolive","redbull","ahava","sabra",
"teradata","fiverr","mondaycom","tabnine","bankhapoalim","tempobeverages",
"barclaysbank","sodastream","subway","kelloggs","kraftheinz","hersheys",
"mars","nestleusa","drpepper","benandjerrys","levi","underarmour",
"calvinklein","tommyhilfiger","ralphlauren","newbalance","converse","vans",
"thenorthface","columbia","guess","michaelkors","esteelauder","maybelline",
"revlon","clinique","maccosmetics","covergirl","neutrogena","olay",
"bathandbodyworks","victoriassecretbeauty","alainwater","alrawabidairy",
"alislamifoods","emiratesrefreshments","alghurairfoods","luluhypermarket",
"splash","maxfashion","shoemart","brandsforless","lifestyleuae",
"mikyajy","thefaceshopuae","nazihgroup","nstylebeautylounge","tipsandtoesspa",
"tesla","spacex","neuralink","theboringcompany","xai","x","starlink",
"korkmaz","vestel","beko","arcelik","simfer","karaca","ulker","pinar",
"colaturka","solen","kent","dimes","coppy"
];

const arabicBrands = {
"كوكاكولا":"cocacola",
"بيبسي":"pepsi",
"نستله":"nestle",
"ستاربكس":"starbucks",
"ماكدونالدز":"mcdonalds",
"برجر كينغ":"burgerking",
"كي إف سي":"kfc",
"دومينوز بيتزا":"dominospizza",
"بيتزا هت":"pizzahut",
"أونيلفر":"unilever",
"بروكتر آند غامبل":"proctergamble",
"دانون":"danone",
"لوريال":"loreal",
"زارا":"zara",
"إتش آند إم":"hm",
"نايكي":"nike",
"أديداس":"adidas",
"بوما":"puma",
"أمازون":"amazon",
"ديزني":"disney",
"نتفليكس":"netflix",
"شل":"shell",
"إتش بي":"hp",
"هيوليت باكارد":"hewlettpackard",
"إنتل":"intel",
"جوجل":"google",
"فيسبوك":"meta",
"باي بال":"paypal",
"جونسون آند جونسون":"johnsonandjohnson",
"كولجيت":"colgatepalmolive",
"ريد بول":"redbull",
"أهافا":"ahava",
"صبرا":"sabra",
"أولكر":"ulker",
"بكو":"beko",
"أرسليك":"arcelik"
};

const texts = {
tr:{header:"Boykot Bilinci: Mazlumlara Umut Ol",description:"Bir markayı yaz ve boykot listesinde olup olmadığını öğren:",placeholder:"Marka adını yaz...",button:"Kontrol Et",empty:"Lütfen bir marka adı gir."},
en:{header:"Boycott Awareness: Give Hope to the Oppressed",description:"Type a brand to see if it is on the boycott list:",placeholder:"Enter brand name...",button:"Check",empty:"Please enter a brand name."},
ar:{header:"وعي المقاطعة: أمل للمظلومين",description:"اكتب اسم العلامة التجارية لمعرفة ما إذا كانت ضمن قائمة المقاطعة:",placeholder:"أدخل اسم العلامة التجارية...",button:"تحقق",empty:"الرجاء إدخال اسم العلامة التجارية."}
};

function normalizeString(str){
return str.toLowerCase()
.replace(/ı/g,'i').replace(/İ/g,'i')
.replace(/ş/g,'s').replace(/Ş/g,'s')
.replace(/ç/g,'c').replace(/Ç/g,'c')
.replace(/ü/g,'u').replace(/Ü/g,'u')
.replace(/ö/g,'o').replace(/Ö/g,'o')
.replace(/ğ/g,'g').replace(/Ğ/g,'g')
.replace(/[^a-z0-9]/gi,'');
}

function changeLanguage(){
const lang=document.getElementById("language").value;
document.getElementById("headerText").textContent=texts[lang].header;
document.getElementById("descriptionText").textContent=texts[lang].description;
document.getElementById("brandInput").placeholder=texts[lang].placeholder;
document.querySelector("button").textContent=texts[lang].button;
}

function checkBrand(){
let input=document.getElementById("brandInput").value.trim();
const lang=document.getElementById("language").value;

if(!input){
document.getElementById("result").innerHTML=texts[lang].empty;
return;
}

if(lang==="ar" && arabicBrands[input]){
    input=arabicBrands[input];
}

input=normalizeString(input);

if(boykotMarkalar.includes(input)){
    document.getElementById("result").innerHTML=`⚠️ ${input.toLowerCase()} BOYKOT edilmektedir. SAVAŞA MAHKUM BIRAKILAN MÜSLÜMAN KARDEŞLERİMİZİ UNUTMAYALIM!!!`;
}else{
    document.getElementById("result").innerHTML=`✅ ${input.toLowerCase()} markası şu anda boykot listesinde yer almıyor.`;
}
}
</script>
</body>
</html>
