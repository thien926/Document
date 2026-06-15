# Collection cơ bản

## 💡 Quy tắc tiền tố

* **Array-**: Sử dụng mảng (array) để lưu dữ liệu.
* **Linked-**: Sử dụng danh sách liên kết (linked list). Giữ nguyên thứ tự thêm vào.
* **Hash-**: Sử dụng bảng băm (hash table), không có thứ tự, tốc độ nhanh nhất.
* **Tree-**: Sử dụng cấu trúc cây (thường là Red-Black Tree). Tự động sắp xếp tăng dần.

---

## ArrayList vs LinkedList (Nhóm List)

| ArrayList                            | LinkedList                                 |
| ------------------------------------ | ------------------------------------------ |
| Truy cập phần tử rất nhanh nhờ index | Truy cập phần tử chậm do phải duyệt từ đầu |
| Chèn hoặc xóa ở giữa danh sách chậm  | Chèn hoặc xóa phần tử rất nhanh            |

---

## HashMap vs LinkedHashMap vs TreeMap (Nhóm Map - Key/Value)

* **HashMap**

  * Tốc độ nhanh nhất.
  * Thứ tự xuất ra ngẫu nhiên.

* **LinkedHashMap**

  * Giữ nguyên thứ tự thêm vào.

* **TreeMap**

  * Tự động sắp xếp các Key theo thứ tự tăng dần.

---

## HashSet vs LinkedHashSet vs TreeSet (Nhóm Set - Không trùng lặp)

* **HashSet**

  * Tốc độ nhanh nhất.
  * Thứ tự phần tử ngẫu nhiên.

* **LinkedHashSet**

  * Giữ nguyên thứ tự phần tử khi thêm vào.

* **TreeSet**

  * Tự động sắp xếp các phần tử tăng dần.

---

# Ví dụ

```java
// List
List<String> arrayList = new ArrayList<>();
List<String> linkedList = new LinkedList<>();

// Map
Map<Integer, String> hashMap = new HashMap<>();
Map<Integer, String> linkedHashMap = new LinkedHashMap<>();
Map<Integer, String> treeMap = new TreeMap<>();

// Set
Set<Integer> hashSet = new HashSet<>();
Set<Integer> linkedHashSet = new LinkedHashSet<>();
Set<Integer> treeSet = new TreeSet<>();
```
