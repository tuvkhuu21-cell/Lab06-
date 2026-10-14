# Лабораторийн ажил №6

Хичээл: F.CSA313 — Программ хангамжийн чанарын баталгаа ба тест

Сэдэв: Category-Partition ба PICT

Оюутны нэр: Түвшинтөр
Оюутны код: B190910820			

## Хэсэг A: Category-Partition

### A1. Сонголт ба төлөөлөх утгууд

| Category | Choice | Representative value |
|---|---|---|
| From | Valid | from=1001, active account |
| From | Frozen | from=1002, frozen account |
| From | Missing | from=9999, account does not exist |
| To | Valid | to=2001, existing different account |
| To | Missing | to=9998, account does not exist |
| To | SameAsFrom | to=1001, same as source |
| Amount | Zero | amount=0 |
| Amount | Normal | amount=50000 |
| Amount | OverBalance | amount=1500000, balance=1000000 |
| Amount | OverLimit | amount=600000, remaining daily limit=500000 |
| Currency | Same | MNT to MNT |
| Currency | Different | USD transfer from MNT account |

### A2. Хязгаарлалтууд

| Category | Choice | Constraint | Reason |
|---|---|---|---|
| From | Valid | - | Хүчинтэй эх данс |
| From | Frozen | [ERROR] | Царцаагдсан данс |
| From | Missing | [ERROR] | Эх данс байхгүй |
| To | Valid | - | Хүчинтэй хүлээн авагч |
| To | Missing | [ERROR] | Хүлээн авагч байхгүй |
| To | SameAsFrom | [ERROR] | Ижил данс руу шилжүүлэх |
| Amount | Zero | [ERROR] | Дүн 0 байх боломжгүй |
| Amount | Normal | - | Зөвшөөрөгдөх дүн |
| Amount | OverBalance | [ERROR] | Үлдэгдэл хүрэлцэхгүй |
| Amount | OverLimit | [ERROR] | Өдрийн лимит хэтэрсэн |
| Currency | Same | - | Хөрвүүлэлт шаардлагагүй |
| Currency | Different | [SINGLE] | Ханшийн хөрвүүлэлтийг тусад нь шалгах |

### Алдааны давуу эрэмбэ

From → To → Amount

Нэг хүсэлтэд олон алдаа байвал эхлээд From,
дараа нь To, хамгийн сүүлд Amount-ийн алдааг
буцаана гэж үзсэн.

Энэ нь тест дизайн хийх зорилгоор сонгосон
таамаглал бөгөөд API-ийн тодорхойлолтод
заагаагүй.

### A2. IF нөхцөлүүд

1. IF To = SameAsFrom THEN From данс байх ёстой.
2. IF Amount = OverBalance THEN үлдэгдэл хүрэлцэхгүй.
3. IF Amount = OverLimit THEN өдрийн үлдсэн лимит хүрэлцэхгүй.
4. IF Currency = Different THEN ханшийн хөрвүүлэлт хийнэ.

### Далд орчны нөхцөл

Өдрийн лимитийн өмнөх ашиглалт нь
шилжүүлэг амжилттай болох эсэхэд нөлөөлнө.

Жишээ:
- Өдрийн лимит: 1,000,000 MNT
- Өмнө ашигласан: 500,000 MNT
- Үлдсэн лимит: 500,000 MNT

### A2. Хослолын тооцоо

Хязгаарлалтгүй бүрэн олонлог:

N = 3 × 3 × 4 × 2 = 72

[ERROR] сонголтуудыг тусгаарлан шалгах,
[SINGLE] сонголтыг нэг удаа ашиглах
стратеги сонгосон.

Хязгаарлалтын дараах M тоог
PICT моделийн нөхцөлүүдтэй уялдуулан
эцэслэж тооцно.

A3 хүснэгтэд 10 тест боловсруулсан.
TC10 нь олон алдаатай тусгай тест юм.

### A3. Тестийн спецификаци

| ID | From | To | Amount | Currency | Expected |
|---|---|---|---|---|---|
| TC01 | Valid | Valid | Normal | Same | 201 OK |
| TC02 | Valid | Valid | Normal | Different | 201 OK |
| TC03 | Missing | Valid | Normal | Same | 200 ERROR_NO_FROM |
| TC04 | Frozen | Valid | Normal | Same | 200 ERROR_FROZEN |
| TC05 | Valid | Missing | Normal | Same | 200 ERROR_NO_TO |
| TC06 | Valid | SameAsFrom | Normal | Same | 200 ERROR_SAME_ACCOUNT |
| TC07 | Valid | Valid | Zero | Same | 200 ERROR_BAD_AMOUNT |
| TC08 | Valid | Valid | OverBalance | Same | 200 ERROR_INSUFFICIENT |
| TC09 | Valid | Valid | OverLimit | Same | 200 ERROR_LIMIT |
| TC10 | Missing | Missing | Zero | Same | 200 ERROR_NO_FROM |

## Хэсэг B: PICT
### B1. Browser model — Pairwise Testing

Browser model нь дараах 6 параметртэй:

- Content: Allow, Restrict, Block
- Cookies: Allow, Restrict, Block
- Popups: Yes, No
- Addons: Yes, No
- Attacks: Yes, No
- Forgeries: Yes, No

Бүрэн олонлог:

N = 3 × 3 × 2 × 2 × 2 × 2 = 144

### PICT үр дүн

| Testing method | Test cases |
|---|---:|
| Full combination | 144 |
| PICT 2-way | 10 |
| PICT 3-way | 20 |
| Lecture manual 2-way | 9 |

### Үр дүнгийн тайлбар

PICT 2-way тестээр 10 тест үүссэн бөгөөд
лекц дээр гараар боловсруулсан 9 тестээс
нэгээр олон байна.

Учир нь PICT нь greedy алгоритм ашигладаг.
Энэ алгоритм нь параметрүүдийн хослолыг
хамруулах боловч хамгийн цөөн тестийн
тоог заавал гаргах баталгаа өгдөггүй.

3-way тестээр 20 тест үүссэн.
Энэ нь 2-way тестээс олон боловч
бүрэн олонлогийн 144 тестээс цөөн байна.

## Дүгнэлт
