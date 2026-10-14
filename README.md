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

### Бүрэн олонлог

N = 3 × 3 × 4 × 2 = 72

N → M = 72 → (A2 хязгаарлалтыг эцэслэсний дараа тооцно)
## Хэсэг B: PICT

## Дүгнэлт
