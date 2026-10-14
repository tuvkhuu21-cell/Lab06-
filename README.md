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
| Currency | Different | [SINGLE] | Валютын хөрвүүлэлтийн тусгай замыг нэг удаа тусгаарлан шалгаж, бусад параметрийг хүчинтэй утгатай байлгана |

### Алдааны давуу эрэмбэ

From → To → Amount

Нэг хүсэлтэд олон алдаа байвал эхлээд From,
дараа нь To, хамгийн сүүлд Amount-ийн алдааг
буцаана гэж үзсэн.

Энэ нь тест дизайн хийх зорилгоор сонгосон
таамаглал бөгөөд API-ийн тодорхойлолтод
заагаагүй.

### A2. IF нөхцөлүүд

Алдааг тусгаарлан шалгахын тулд дараах
хязгаарлалтуудыг сонгов.

1. IF From = Missing THEN To = Valid AND Amount = Normal.
2. IF From = Frozen THEN To = Valid AND Amount = Normal.
3. IF To = Missing OR To = SameAsFrom THEN From = Valid AND Amount = Normal.
4. IF Amount = Zero OR Amount = OverBalance OR Amount = OverLimit
   THEN From = Valid AND To = Valid.

Эдгээр нь API-ийн заавал мөрдөх бизнес дүрэм биш,
харин нэг тестэд нэг алдааг тусгаарлах зорилгоор
сонгосон тест дизайны хязгаарлалтууд юм.

TC10 нь олон алдаа давхацсан үед алдааны
давуу эрэмбийг шалгах тусгай тест тул
эдгээр хязгаарлалтаас зориуд гадуур байна.

### Далд орчны нөхцөл

Өдрийн лимитийн өмнөх ашиглалт нь
шилжүүлэг амжилттай болох эсэхэд нөлөөлнө.

Жишээ:
- Өдрийн лимит: 1,000,000 MNT
- Өмнө ашигласан: 500,000 MNT
- Үлдсэн лимит: 500,000 MNT

### A2. Хослолын тооцоо

Хязгаарлалтгүй үед:

N = 3 × 3 × 4 × 2 = 72

[ERROR] сонголтуудыг тус тусад нь шалгаж,
[SINGLE] сонголтыг нэг удаа тусгайлан шалгахаар сонгосон.

IF нөхцөлүүдийг хэрэглэсний дараа:

- From=Valid, To=Valid: 4 × 2 = 8
- From=Valid, To=Missing/SameAsFrom: 2 × 1 × 2 = 4
- From=Frozen/Missing, To=Valid: 2 × 1 × 2 = 4

M = 8 + 4 + 4 = 16

PICT-ээр хязгаарлалттай модель ажиллуулахад
14 тест гарсан. Харин M=16 нь IF нөхцөлийг
хангасан бүх боломжит хослолын тоо юм.

A3 хэсэгт нийт 10 тест бичсэн.
TC10 нь олон алдаа давхацсан үед
аль алдааг түрүүлж буцаахыг шалгана.

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

### B2. Transfer model — PICT

Банкны шилжүүлгийн 3 модель үүсгэж,
PICT-ээр 2-way тест гаргасан.

| Model | Test cases | Happy path | Max errors |
|---|---:|---:|---:|
| transfer-nc.model | 13 | 1 | 3 |
| transfer.model | 14 | 0 | 1 |
| transfer-neg.model | 16 | 2 | 1 |

transfer-nc.model нь хязгаарлалтгүй.
Нэг тестэд хэд хэдэн алдаа давхцаж болно.

transfer.model-д IF нөхцөлүүд нэмсэн.
Ингэснээр алдаануудыг тус тусад нь
шалгах боломжтой болсон.

transfer.model-ийн гаралтад happy path
мөр гараагүй. Гэхдээ IF нөхцөлүүд
амжилттай шилжүүлгийг хориглоогүй.

transfer-neg.model-д алдаатай утгуудыг
~ тэмдэглэгээгээр ялгасан.
Үүссэн тестүүдэд нэг мөрөнд хоёр
negative утга давхцаагүй.

transfer-neg.model-д 2 happy path мөр гарсан.
~ тэмдэглэгээ нь нэг тестэд олон алдаа
давхцахаас сэргийлдэг боловч бүх
бизнес дүрмийг орлохгүй.

Хязгаарлалт нэмсэн ч тестийн тоо
заавал багасдаггүй. Миний ажиллуулсан
PICT-ийн үр дүнд 13, 14, 16 тест гарсан.

### B2. Бодит тест кейсүүд

#### TC01 — Амжилттай шилжүүлэг

Бэлтгэл:
- 1001 данс идэвхтэй, үлдэгдэл 1,000,000 MNT.
- 2001 данс байгаа, 1001-ээс өөр.
- Өдрийн үлдсэн лимит 500,000 MNT.

Request:
POST /transfer

    {
      "from": 1001,
      "to": 2001,
      "amount": 50000,
      "currency": "MNT"
    }

Expected:
HTTP 201

    {"result":"OK"}

#### TC02 — Эх данс байхгүй

Бэлтгэл:
- 9999 дугаартай данс байхгүй.
- 2001 данс байгаа.

Request:
POST /transfer

    {
      "from": 9999,
      "to": 2001,
      "amount": 50000,
      "currency": "MNT"
    }

Expected:
HTTP 200

    {"result":"ERROR_NO_FROM"}

#### TC03 — Өдрийн лимит хэтэрсэн

Бэлтгэл:
- 1001 данс идэвхтэй, үлдэгдэл 1,000,000 MNT.
- 2001 данс байгаа.
- Өдрийн үлдсэн лимит 500,000 MNT.

Request:
POST /transfer

    {
      "from": 1001,
      "to": 2001,
      "amount": 600000,
      "currency": "MNT"
    }

Expected:
HTTP 200

    {"result":"ERROR_LIMIT"}

## Дүгнэлт

Лабораторийн ажил 6-аар Category-Partition болон
PICT ашиглан тестийн тохиолдол боловсруулж сурлаа.

Category-Partition аргаар тест хийхэд эхлээд
ямар нөхцөлд алдаа гарахыг бодож тодорхойлох
шаардлагатай байсан.

Гараар тест зохиох нь бизнес дүрэм болон
алдааны шалтгааныг ойлгоход илүү тохиромжтой.

Харин PICT нь олон параметрийн хослолыг
автоматаар гаргаж өгдөг тул цаг хэмнэдэг.

PICT-ийн 2-way арга нь параметрүүдийн
боломжит хос бүрийг хамруулахад тусалдаг.

Гэхдээ PICT нь аль алдаа түрүүлж гарахыг
өөрөө мэдэхгүй учраас хязгаарлалт зөв
тодорхойлох шаардлагатай.

Хязгаарлалтгүй модельд хэд хэдэн алдаа
нэг тестэд давхцаж байгааг харсан.

Negative утгад ~ тэмдэглэгээ хэрэглэснээр
алдаануудыг тусгаарлаж шалгах боломжтой болсон.

Миний хувьд хамгийн их бодол шаардсан хэсэг
нь IF нөхцөлүүдийг тодорхойлох болон
давхар алдааны хүлээгдэх үр дүнг сонгох байлаа.
