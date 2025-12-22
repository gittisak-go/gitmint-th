# gitmint-th 
Press enter or click to view image in full size

เมื่ออาทิตย์ก่อนผมได้ข่าว ธนาคารไทยพาณิชย์เปิด API ให้นักพัฒนาภายนอก สมัครใช้งานใน sandbox ได้ทันที, มี simulator ให้จำลองธุรกรรมจริง ผมลองเล่น ปรากฏว่า api ใช้งานได้ มี response เป็น Deep link Url แต่ก็ใช้ไม่ได้เพราะยังไม่มี Simulator App ของ SCB easy และไม่กี่วันก่อนมีข่าว กสิกรเปิด API ให้คนนอกเข้าถึงแอพ K PLUS เริ่มจาก QR Payment เตรียมเปิด API ขอ statement เมื่อแปลงเป็น qr code ระบบแจ้งว่าไม่พบร้านค้า กลับมาที่ Prompt-pay QR code ก็ในเมื่อผมทำ QR code ของกสิกรไทยไม่ได้แล้ว วันนี้ผมจะมาแนะนำวิธีทำ Prompt-pay QR code แบบง่าย ๆ กันครับ
promptpay-qr
promptpay-qr เป็น package ที่สามารถทำ prompt pay qrcode ที่ใช้จ่ายผ่านแอปพลิเคชันของธนาคาร โดยใช้เบอร์โทรศัพท์หรือเลขบัตรประชาชน อีกทั้งยังกำหนดจำนวนเงินที่ต้องชำระเงินได้ด้วย ผมอาจจะอธิบายไม่เห็นภาพ เพื่อที่จะเข้าใจมากขึ้น สามารถไปลองเล่นได้ที่ https://promptpay2.me ครับ
promptpay-qr จะ generate เป็น string ถ้านำไปแสกนโดยผ่านแอปพลิเคชันแสกน QR Code ทั่วไป ผลลัพธ์ที่ได้ จะเป็นการค้นหา string ผ่านเบราเซอร์ ดังนั้น ต้องใช้แอปพลิเคชันของธนาคารเท่านั้นถึงจะแสกนได้ ส่วน string ที่ generate มาจากไหน หรืออยากจะลองทำขึ้นมาเอง ก็ศึกษาได้จากลิ้งนี้เลยครับ มีอะไรอยู่ใน PromptPay QR แกะสเปค QR ที่จะใช้จ่ายผ่าน mobile banking ได้ทุกธนาคารในอนาคต
Get Thanainan Khamthaen’s stories in your inbox
Join Medium for free to get updates from this writer.

Subscribe
promptpay-qr สามารถใช้งานได้ 2 แบบ แบบแรก คือ ใช้งานผ่าน Terminal หรือ Command line แบบที่สอง ใช้งานผ่าน JavaScript API ขั้นตอนต่อไปมาลองเล่นกันครับ
สิ่งที่ควรรู้
พื้นฐานภาษา javascript, node.js
สิ่งที่ต้องมีในเครื่อง
Node.js ใน Node.js จะ npm ใช้สำหรับลง package ของ javascript
Code editor เช่น Visual Studio Code
แบบที่ 1 : ใช้งานผ่าน Terminal
Step 1 : Generate a promptpay-qr
สำหรับเบอร์โทรศัพท์ พิมพ์คำสั่งด้านล่างเพื่อ generate promptpay-qr รูปแบบเบอร์โทรศัพท์ต้องเป็นรูปแบบ xxx-xxx-xxxxx เท่านั้น
npx promptpay-qr 000-000-0000
สำหรับเลขบัตรประชาชน พิมพ์คำสั่งด้านล่างเพื่อ generate promptpay-qr รูปแบบเลขบัตรประชาชนต้องเป็นรูปแบบ x-xxxx-xxxxx-xx-x เท่านั้น
npx promptpay-qr 0-0000-00000-00-0
สามารถกำหนดจำนวนเงินด้วยคำสั่ง--amount x.xx
npx promptpay-qr 0-0000-00000-00-0 --amount 4.22
Press enter or click to view image in full size

ภาพจาก https://raw.githubusercontent.com/dtinth/promptpay-qr/HEAD/images/terminal.png
แค่นี้ก็ได้ promptpay-qr ผ่าน terminal แล้วครับ
Step 2: Scan promptpay-qr by mobile banking
แสกน promptpay-qr ผ่านแอปพลิเคชันธนาคาร
Press enter or click to view image in full size

จากการทดสอบpromptpay-qr สามารถแสกนได้เลยครับ
แบบที่ 2 : เรียก promptpay-qr ผ่าน javascript api
Step 1 : Create package.json and install dependencies
ทำการสร้างโฟล์เดอร์ชื่อ test-promptpay-qr และเข้าไปที่ directory ./test-promptpay-qr
mkdir test-promptpay-qr && cd test-promptpay-qr
จากนั้นพิมพ์คำสั่ง npm init เพื่อสร้างไฟล์ package.json จะมีให้กรอกชื่อ package name: ขั้นตอนนี้ให้กดปุ่ม Enter รัว ๆ เลย เพราะสามารถมาแก้ไขทีหลังได้ครับ
Press enter or click to view image in full size

ทำการลง package : promptpay-qrใช้สำหรับ generate promptpay
npm install promptpay-qr
ทำการลง package : qrcode ใช้สำหรับแปลง string เป็น qrcode
npm install qrcode
ทำการลง package : fs ใช้สำหรับเขียนไฟล์ .svg
npm install fs
Step 2 : Coding

ก่อนอื่นเลยผมขออธิบายโค้ดก่อนนะครับ
บรรทัดที่ 1 ถึง 3 เป็นการ import package เข้ากับตัวเแปรเพื่อนำไปใช้งาน
บรรทัดที่ 5 และ 6 เป็นการกำหนดตัวแปรเบอร์โทรศัพท์และเลขบัตรประชาชน โดยที่รูปแบบต้องเป็น xxx-xxx-xxxxx และ x-xxxx-xxxxx-xx-x ตามลำดับเท่านั้น
บรรทัดที่ 7 เป็นการกำหนดตัวแปรจำนวนเงิน ถ้า amount = 0 ผู้ใช้งานสามารถกรอกจำนวนเงินที่ต้องจ่ายได้ ส่วนถ้าเป็นตัวเลขอื่นที่มากกว่า 0 เช่น amount = 10 ก็คือ ผู้ใช้ไม่สามารถกรอกได้ เลขจำนวนเงินจะกำหนดที่ 10
บรรทัดที่ 8 เป็นการเรียกใช้ฟังก์ชัน generatePayload() โดยที่ parameter ตัวแรกจะส่ง mobileNumber หรือ IDCardNumber และ parameter ตัวที่สองจะส่ง amount ในรูปแบบ json หรือ { } ผลลัพธ์จะได้ค่า payload เป็น 00020101021129370016A000000677010111011300660000000005802TH530376463048956
บรรทัดที่ 12 เป็นการกำหนด option ของ qrcode โดยที่ type: `svg` (type สามารถเป็น terminal ได้ ถ้าต้องการแสดง QR Code ผ่าน terminal ตัวรูปแบบ QR Code จะมีสองสี คือ สีเข้มกับสีสว่าง ในทีนี้ผมให้เป็นสีขาวกับสีดำ
บรรทัดที่ 13 ถึง 17 เป็นการเรียกใช้ฟังก์ชัน qrcode.toString() โดยที่ส่งค่า payload และ options ไปด้วย จากนั้นเมื่อฟังก์ชันนี้ทำงานเสร็จแล้ว ถ้ามี err ทำการ console.log(err) ถ้าไม่ error ให้ทำการเขียนไฟล์ qr.svg ด้วยฟังก์ชัน fs.writeFileSync()
ทำการสร้างไฟล์ชื่อ index.js จากนั้นคัดลอกโค้ดด้านบนมาใส่เลยครับ
Step 3 : Run node
พิมพ์คำสั่งด้านล่างเพื่อใช้งาน node
node index.js
จะได้แบบนี้ครับ
Press enter or click to view image in full size

Step 4 : Scan promptpay-qr by mobile banking
ทำการเปิดไฟล์ qr.svg
Press enter or click to view image in full size

จากนั้นทำการทดสอบด้วยแอปพลิเคชันธนาคาร
Press enter or click to view image in full size

แค่นี้ก็สามารถใช้งานได้แล้วครับ
สำหรับบทความนี้ก็มีเพียงเท่านี้ครับ ผมคิดว่า Prompt-pay QR code สามารถต่อยอดได้เยอะเลย ไม่ว่าจะใช้กับการเงินต่าง ๆ หนึ่งอย่างที่ผมคิดว่าควรนำมาใช้ ก็คือ การจ่ายค่าหอครับ แสกนแล้วกดจ่ายเลย ไม่จำเป็นต้องมากรอกบัญชี จำนวนเงิน ให้เสียเวลา ฮ่าๆ และก็นอกจาก javascript แล้ว promptpay-qr ยังมีของ php ด้วยครับ ก็ไปลองเล่นกันนะครับผม