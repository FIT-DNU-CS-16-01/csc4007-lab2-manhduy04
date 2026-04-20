1. Data audit
Số lượng mẫu đã dùng: 50,000
Phân bố nhãn positive / negative:
positive: 25,000
negative: 25,000
Độ dài review điển hình:
median: ~173–207 từ
p95: ~590–705 từ
Có missing / empty text không?
Không (missing = 0)
Có duplicate không?
Có (~1.6%)
2 quan sát đáng chú ý về dữ liệu IMDB:
Dataset cân bằng hoàn toàn
Bằng chứng: 25k positive / 25k negative
Ý nghĩa: giúp đánh giá model công bằng, accuracy ≈ macro-F1
Xử lý: không cần can thiệp
Độ dài review rất không đồng đều
Bằng chứng: từ 4 → ~2800 từ
Ý nghĩa: review dài dễ chứa nhiều cảm xúc trái chiều
Xử lý: giữ nguyên nhưng chú ý khi phân tích lỗi
Có dữ liệu trùng lặp (~1.6%)
Ý nghĩa: có thể gây overfitting
Xử lý: có thể loại bỏ, nhưng trong lab giữ nguyên
2. Preprocessing design
Các bước làm sạch đã dùng:
Lowercase
Loại bỏ HTML tags
Chuẩn hóa text cơ bản
Giữ lại dấu câu hay bỏ? Vì sao?
→ Giữ lại dấu câu
Vì dấu câu (!!!, ???) mang tín hiệu cảm xúc mạnh
Nếu xoá có thể làm giảm hiệu quả model
Có thay số bằng <NUM> không? Vì sao?
→ Không
Vì các pattern như “10/10”, “1/10” chứa sentiment rõ ràng
Có bước nào cố tình không làm?
→ Không xoá toàn bộ punctuation
→ Không xử lý quá mạnh tay
Vì có thể làm mất tín hiệu cảm xúc
3. Experiment comparison
| Run | Text version | Vectorizer | Model | ngram | Macro-F1 | Accuracy | Ghi chú |
|-----|--------------|------------|-------|-------|----------|----------|---------|
| 1 | cleaned | TF-IDF | Logistic Regression | bigram | 0.9068 | 0.9068 | baseline |
| 2 | cleaned | TF-IDF | Linear SVM | bigram | 0.9098 | 0.9098 | tốt nhất |
| 3 | cleaned | BoW | Logistic Regression | bigram | ~0.87 | ~0.87 | kém hơn |
4. Error analysis (>= 10 lỗi)
Nhóm lỗi chính:
Phủ định (negation)
Cảm xúc trộn lẫn (mixed sentiment)
Mỉa mai (sarcasm)
Review dài
| ID | True | Pred | Nhóm lỗi | Giải thích ngắn |
|-----|------|------|----------|----------------|
| 1 | positive | negative | phủ định | có "not good" gây hiểu sai |
| 2 | negative | positive | mixed | vừa khen vừa chê |
| 3 | positive | negative | sarcasm | câu mỉa mai |
| 4 | negative | positive | phủ định | "not bad" bị hiểu sai |
| 5 | positive | negative | dài | nhiều ý trái chiều |
| 6 | negative | positive | mixed | sentiment không rõ ràng |
| 7 | positive | negative | sarcasm | dùng irony |
| 8 | negative | positive | dài | thông tin bị loãng |
| 9 | positive | negative | phủ định | từ khóa gây nhiễu |
| 10 | negative | positive | mixed | câu phức |
5. Reflection
Pipeline tốt nhất:
→ TF-IDF + Linear SVM
Vì xử lý tốt dữ liệu sparse
Giảm ảnh hưởng từ phổ biến
Accuracy và Macro-F1 có chênh không?
→ Không đáng kể (0.9098)
Vì dataset cân bằng
Nếu dữ liệu lệch lớp?
→ Macro-F1 quan trọng hơn
Vì accuracy có thể gây hiểu sai
Cải tiến cho Lab 3:
Dùng bigram (n-gram > 1)
Hoặc dùng deep learning (LSTM, BERT)
🎯 KẾT LUẬN
Model đạt 91% → baseline tốt
Dataset sạch → không cần xử lý phức tạp
Lỗi chủ yếu do:
không hiểu ngữ cảnh
không xử lý tốt phủ định