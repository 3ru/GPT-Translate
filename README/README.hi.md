# 🌎 मार्कडाउन अनुवाद बॉट
[![Maintainability](https://api.codeclimate.com/v1/badges/a13ea4f37913ba6ba570/maintainability)](https://codeclimate.com/github/3ru/gpt-translate/maintainability)
[![GPT Translate](https://github.com/3ru/gpt-translate/actions/workflows/gpt-translate.yml/badge.svg)](https://github.com/3ru/gpt-translate/actions/workflows/gpt-translate.yml)

[English](/README.md) |
[简体中文](/README/README.zh-CN.md) |
[繁體中文](/README/README.zh-TW.md) |
[Español](/README/README.es.md) |
[हिंदी, हिन्दी](/README/README.hi.md) |
[한국어](/README/README.ko.md) |
[日本語](/README/README.ja.md)

यह GitHub एक्शन आपके मार्कडाउन फाइलों को GPT-4, GPT-3.5 मॉडल का उपयोग करके कई भाषाओं में अनुवाद करता है।

> [!Important]  
> OpenAI API वर्तमान में मुफ्त में उपलब्ध नहीं है। इस वर्कफ़्लो का उपयोग करने के लिए आपको एक `पेड अकाउंट` के साथ जारी किया गया API Key चाहिए।  
> <img width="387" alt="image" src="https://github.com/3ru/gpt-translate/assets/69892552/8c803edb-85ef-41ee-a4be-be52b3a30eba">

<br/>

<details><summary>🧐 वर्तमान स्थिति</summary>
<p>

- यह एक्शन केवल **मार्कडाउन(`.md`), मार्कडाउन-jsx(`.mdx`), json(`.json`) फाइलों** का अनुवाद करता है।

- यह कमांड केवल उन व्यक्तियों द्वारा निष्पादित की जा सकती है जिनके पास **रिपॉजिटरी पर लिखने की अनुमति** है।

ये सीमाएं गैर-विश्वसनीय पार्टियों द्वारा API के दुरुपयोग को रोकती हैं।

</p>
</details> 

## 🔧 सेटअप

### रिपॉजिटरी सेटिंग्स

#### 1. सेटिंग्स > एक्शन्स > जनरल

- `Read and write permissions` सक्षम करें
- `Allow GitHub Actions to create and approve pull requests` सक्षम करें
  ![permissions](https://user-images.githubusercontent.com/69892552/228692074-d8d009a8-9272-4023-97b1-3cbc637d5d84.jpg)

#### 2. सेटिंग्स > सीक्रेट्स और वेरिएबल्स > एक्शन्स

- [अपने API Key](https://platform.openai.com/account/api-keys)(`OPENAI_API_KEY`) को सीक्रेट्स में सेट करें
  ![secrets](https://user-images.githubusercontent.com/69892552/228692421-22d7db33-4e32-4f28-b166-45b4d3ce2b11.jpg)


### GitHub Actions वर्कफ़्लो सेटिंग्स

#### आवश्यक
- OPENAI_API_KEY को apiKey के रूप में प्रदान करें।
- `on` को सेट करें ताकि यह एक टिप्पणी बनाए जाने पर ट्रिगर हो (`types: [ created ]`)।
- पहले से चेकआउट करें(`actions/checkout@v3`)।

#### अनुशंसित (अनावश्यक रन टाइम को कम करने के लिए)
- केवल तब चलाने के लिए कॉन्फ़िगर करें जब टिप्पणी में `/gpt-translate` या `/gt` मौजूद हो।

👇 यहां एक न्यूनतम वर्कफ़्लो उदाहरण है:
```yaml
# .github/workflows/gpt-translate.yml
name: GPT Translate

on:
  issue_comment:
    types: [ created ]

jobs:
  gpt_translate:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Run GPT Translate
        if: |
          contains(github.event.comment.body, '/gpt-translate') || 
          contains(github.event.comment.body, '/gt')
        uses: 3ru/gpt-translate@master
        with:
          apikey: ${{ secrets.OPENAI_API_KEY }}
```


## 💡 उपयोग

```
/gpt-translate [इनपुट फाइलपाथ] [आउटपुट फाइलपाथ] [लक्षित भाषा] 
```
आप /gpt-translate के लिए /gt का संक्षिप्त रूप भी उपयोग कर सकते हैं।

1. एक इश्यू या पुल रिक्वेस्ट में `/gpt-translate` या `/gt` के साथ एक टिप्पणी बनाएं।

2.【इश्यू पर】अनुवादित फाइलें एक **पुल रिक्वेस्ट** के रूप में बनाई जाएंगी।

2.【पुल रिक्वेस्ट पर】अनुवादित फाइलें **नए कमिट के साथ पुल रिक्वेस्ट में जोड़ी जाएंगी**।

दूसरे शब्दों में, यदि आप एक इश्यू पर टिप्पणी करते रहते हैं, तो नए PR लगातार बनाए जाएंगे।
यदि आप एक PR पर टिप्पणी करते रहते हैं, तो नए कमिट लगातार उस PR में जोड़े जाएंगे।

## 📝 उदाहरण
```
/gpt-translate README.md zh-TW/README.md traditional-chinese
```
`README.md` को पारंपरिक चीनी में अनुवादित करें और इसे `zh-TW` डायरेक्टरी के तहत रखें।

### कई फाइलों का समर्थन

आप इनपुट फाइल पथ में वाइल्डकार्ड निर्दिष्ट करके एक बार में कई फाइलों का अनुवाद कर सकते हैं।

यहां एक नमूना है
```
/gpt-translate *.md *.ja.md Japanese
```
यदि रूट डायरेक्टरी में `A.md` और `B.md` हैं, तो आउटपुट `A.ja.md` और `B.ja.md` होगा। फाइल नाम इनपुट फाइलों से विरासत में मिलते हैं।
मैं एक मनमाना फाइल नाम के साथ फाइल आउटपुट करने पर विचार कर रहा हूं, लेकिन यदि आपके पास कोई स्मार्ट विचार है, तो कृपया इसे इश्यू के माध्यम से सुझाएं!

अधिक जानकारी के लिए, कृपया [वेबसाइट](https://g-t.vercel.app/docs/references/path-builder) देखें

## 🌐 समर्थित भाषाएं
**कोई भी भाषा** जिसे GPT-4 या GPT-3.5 द्वारा समझा जा सकता है

## 🏘️ समुदाय
- [चर्चाएं](https://github.com/3ru/gpt-translate/discussions)
  - यदि आपके कोई प्रश्न हैं, तो कृपया GitHub चर्चाओं में पूछने में संकोच न करें :)
- [इश्यू](https://github.com/3ru/gpt-translate/issues)
  - कृपया बग और नई फीचर सुझाव GitHub इश्यू में सबमिट करें

## 📃 लाइसेंस
MIT लाइसेंस