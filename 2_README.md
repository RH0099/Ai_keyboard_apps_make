###### যদি ভালো করে কিছু না বুঝে থাকেন তাহলে#####
আবারো আপনার AI কীবোর্ড অ্যাপটি **Termux**-এ তৈরি করে **APK** আকারে কনভার্ট করার সম্পূর্ণ প্রসেস নিচে দেওয়া হলো।  

---

# **📌 সম্পূর্ণ স্টেপ-বাই-স্টেপ গাইডলাইন**  

### **🔥 ধাপ ১: প্রয়োজনীয় টুলস ইনস্টল করুন**  
আপনার **Termux** আপডেট করুন এবং প্রয়োজনীয় প্যাকেজ ইনস্টল করুন:  
```bash
pkg update && pkg upgrade -y
pkg install python git nano
pip install kivy openai googletrans android buildozer
```
এটি ইনস্টল হয়ে গেলে পরবর্তী ধাপে যান।

---

### **🔥 ধাপ ২: প্রোজেক্ট ফোল্ডার তৈরি করুন**  
প্রোজেক্টের জন্য একটি ফোল্ডার তৈরি করুন এবং সেটিতে যান:  
```bash
mkdir AIKeyboard && cd AIKeyboard
```
এখন **main.py** ফাইল তৈরি করুন:  
```bash
nano main.py
```
📌 **এই ফাইলের মধ্যে নিচের কোডটি লিখুন এবং Ctrl + X চেপে Y লিখে Enter চাপুন।**

```python
from kivy.app import App
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.textinput import TextInput
from kivy.uix.button import Button
from openai import OpenAI
from googletrans import Translator

# OpenAI API Key
openai_api_key = "আপনার_API_কী_এখানে"

# ইনিশিয়ালাইজ AI এবং Translator
translator = Translator()
client = OpenAI(api_key=openai_api_key)

class AIKeyboard(BoxLayout):
    def __init__(self, **kwargs):
        super().__init__(orientation='vertical', **kwargs)

        # Text Input (কীবোর্ড ইনপুট)
        self.text_input = TextInput(hint_text="Type here...", size_hint_y=0.7)
        self.add_widget(self.text_input)

        # AI বাটন
        self.ai_button = Button(text="AI Reply", size_hint_y=0.15)
        self.ai_button.bind(on_press=self.get_ai_response)
        self.add_widget(self.ai_button)

        # ট্রান্সলেট বাটন
        self.translate_button = Button(text="Translate", size_hint_y=0.15)
        self.translate_button.bind(on_press=self.translate_text)
        self.add_widget(self.translate_button)

    def get_ai_response(self, instance):
        user_input = self.text_input.text
        if user_input:
            response = client.completions.create(
                model="text-davinci-003",
                prompt=user_input,
                max_tokens=100
            )
            self.text_input.text = response.choices[0].text.strip()

    def translate_text(self, instance):
        user_input = self.text_input.text
        if user_input:
            translated_text = translator.translate(user_input, dest='en')  # বাংলা থেকে ইংরেজি
            self.text_input.text = translated_text.text

class AIKeyboardApp(App):
    def build(self):
        return AIKeyboard()

if __name__ == "__main__":
    AIKeyboardApp().run()
```
এটি **Ctrl + X → Y → Enter** চেপে সেভ করুন।  

---

### **🔥 ধাপ ৩: অ্যাপ রান করা (পরীক্ষার জন্য)**
```bash
python main.py
```
📌 **আপনার AI কীবোর্ড অ্যাপ চালু হবে** (শুধু Termux-এর মধ্যে)।  

---

## **📌 APK বানানোর প্রসেস**  
এখন আপনার **AI কীবোর্ড অ্যাপকে APK বানাতে হবে।**  

### **🔥 ধাপ ৪: Buildozer সেটআপ করুন**  
```bash
pkg install buildozer
buildozer init
```
📌 এটি একটি **buildozer.spec** ফাইল তৈরি করবে।  

---

### **🔥 ধাপ ৫: buildozer.spec ফাইল এডিট করুন**  
```bash
nano buildozer.spec
```
এখন নিচের লাইনগুলো পরিবর্তন করুন:

```
package.name = AIKeyboard
package.domain = com.yourname.aikeyboard
source.include_exts = py,png,jpg,kv,atlas
requirements = python3,kivy,openai,googletrans
android.permissions = INTERNET
```
📌 **Ctrl + X → Y → Enter** চেপে সেভ করুন।  

---

### **🔥 ধাপ ৬: APK ফাইল তৈরি করুন**  
```bash
buildozer -v android debug
```
📌 এটি একটু সময় নেবে (১৫-৩০ মিনিট)। **সফল হলে `bin/` ফোল্ডারে APK তৈরি হবে।**  

---

## **📌 APK ডাউনলোড ও ইনস্টল করা**
### **🔥 ধাপ ৭: Termux থেকে APK ডাউনলোড করুন**
APK ফাইলটি **bin/** ফোল্ডারে থাকবে। APK-এর নাম চেক করুন:  
```bash
ls bin/
```
ধরুন, APK ফাইলের নাম **AIKeyboard-0.1-debug.apk**  
এখন **Termux থেকে ফোনে পাঠাতে:**  
```bash
mv bin/AIKeyboard-0.1-debug.apk /sdcard/
```
📌 এখন **ফাইল ম্যানেজার** থেকে **APK ইনস্টল করুন।**  

---

## **✅ সম্পূর্ণ প্রসেস শেষ! 🎉**  
### **আপনার AI কীবোর্ড অ্যাপ প্রস্তুত!**  
✅ **কীবোর্ডে AI রিপ্লাই দেবে।**  
✅ **ট্রান্সলেট বাটন কাজ করবে।**  
✅ **APK আকারে ইনস্টলযোগ্য।**  

---

## **🚀 এরপর যেসব করতে পারবেন👇**
✅ **কীবোর্ড ডিজাইন কাস্টমাইজ করতে পারবেন!**  
✅ **Google Play Store-এ আপলোড করতে পারবেন!**  
