# Reflection — Lab 19

**Tên:** Nguyễn Tuấn Kiệt
**Cohort:** A20-K1 
**Path đã chạy:** docker

---

## Câu hỏi (≤ 200 chữ)

> Trên golden set 50 queries, mode nào thắng ở loại query nào (`exact` /
> `paraphrase` / `mixed`), và tại sao? Khi nào bạn **không** dùng hybrid
> (i.e. khi nào pure BM25 hoặc pure vector là lựa chọn đúng)?

**Exact queries:** BM25 và hybrid đều thắng (96.7%) vì chứa từ kỹ thuật verbatim trong corpus. BM25 signal đủ mạnh nên hybrid không cải thiện thêm.

**Paraphrase queries:** Cả hai đều yếu (BM25=33.3%, vector=24.0%, hybrid=32.0%) vì embedding model `bge-small-en-v1.5` được train trên English, không hiểu tốt Vietnamese paraphrases. Đổi sang `bge-m3` multilingual sẽ giúp vector thắng ở đây.

**Mixed queries:** Hybrid thắng rõ (100% vs 97-98%) vì kết hợp cả exact terms và semantic understanding. Đây là pattern thực tế nhất.

**Khi KHÔNG dùng hybrid:**
- Pure BM25: khi cần exact matching (IDs, codes, technical terms), không cần semantic understanding, hoặc latency critical (BM25 nhanh hơn 7x).
- Pure vector: khi queries thuần conceptual/paraphrased và có multilingual model tốt, hoặc cần cross-lingual search.

---

## Điều ngạc nhiên nhất khi làm lab này

PIT join trong Feast (NB4 cell 7) chỉ trả về 2 rows thay vì 3 như expected — user u_001 bị filter ra mặc dù có trong entity_df. Có thể do timestamp (NOW-2h) nằm ngoài TTL window hoặc không có feature data tại thời điểm đó, nhưng Feast không báo warning rõ ràng về missing entities.

---

## Bonus challenge

- [ ] Đã làm bonus (xem `bonus/`)
- [ ] Pair work với: _<tên đồng đội nếu có>_
