### Matching and Proof search

#### Matching__ Unification
**Matching is one of the fundamental ideas in Prolog**
Matching trong prolog ta có thể hiểu là khớpm được đ/n như sau:
1. Nếu term1 và term2 là hằng, thì term1 và term2 khớp khi và chỉ khi chúng cùng kiểu dữ liệu
2. Nếu term1 là biến và term2 là biến bất kỳ thì term1 và term2 khớp và term 1 và term2 được gán giá trị của term1.
3. Nếu term1 và term2 là biểu thức phức tạp thì chúng khớp khi và chỉ khi:
    1. Chúng cùng phương thức (functor) và cùng cấp (arity)
    2. Mọi đối số đều khớp
    3. Các giá tri biến gán phải tương thích.
4. 2 term khớp khi và chỉ khi nó khớp với các điều kiện phía trên

**Phần từ '*=*'**
Phần tử '=/2' là một quy tắc được xây dựng sẵn trong Prolog (/2 phí sau dấu '=' để thể hiện quy tắc lấy 2 đối số)
VD:
```prolog
=(mia,mia).
```
Ngoài ra ta vẫn có thể sử dụng ký tự '=' như các ngôn ngữ khác :
vd: mia=mia
**Đặc biệt :**
Khi ta so sánh 2 biến ngẫu nhiên:
```prolog
X = Y
```
Tùy theo môi trường Prolog mà ta sử dụng prolog có thể trả về *yes*.
Prolog sẽ đồng ý rằng X và Y lúc này đều chỉ về cùng 1 đối tượng. Tức nếu X thay đổi thì Y cũng thay đổi.
Ngoài ra ta có thể nhận được output như sau:
```
X= _5071
Y= _5071
```
Polog tạo ra một biến ngẫu nhiên và gán giá trị của biến đó cho X và Y,  khiến cho X và Y khớp nhau.

## Example
```prolog
vertical(line(point(X,Y),point(X,Z))). 
horizontal(line(point(X,Y),point(Z,Y))).
```
Giải thích:
point/2 : để chỉ tọa độ, X là hoành dộ, Y là tung độ.
line/2 : để chỉ đường kẻ giữa 2 tọa độ
Từ đây ta có thể hiểu
vertical/1 : là 1 đường đường thẳng với hoành độ X giống nhau là 'vertical'
horizontal/1: là 1 đường thẳng với tung độ Y giống nhay là 'horizontal'
Câu truy vấn:
```prolog
horizontal(line(point(2,3),P)).
```
Có nghĩa là ta đang hỏi Prolog giữa tọa độ (2,3) và 1 điểm bất kỳ có thể có 'horizontal' được không.
Prolog sẽ trả về bất kỳ điểm nào có tọa độ y là 3:
```prolog
P = point(_1972,3)
```
_"_1972" là biến ngẫu nhiên được Prolog tạo ra, để chỉ bất kỳ biến X ngẫu nhiên nào cũng được

**Cái này ko biết nên dịch như nào có thể bỏ qua hoặc giữ nguyên tiếng anh (Khá quan trọng)**
>A general remark: the answer to our last query, point(1972,3), is structured. That is, the answer is a complex term, representing a sophisticated concept (namely ‘any point whose y-coordinate is 3’). This structure was built using matching and nothing else: no logical inferences (and in particular, no uses of modus ponens) were used to produce it. Building structure by matching turns out to be a powerful idea in Prolog programming, far more powerful than this rather simple example might suggest. Moreover, when a program is written that makes heavy use of matching, it is likely to be extremely efficient.
>This style of programming is particularly useful in applications where the important concepts have a natural hierarchical structure (as they did in the simple knowledge base above), for we can then use complex terms to represent this structure, and matching to access it. This way of working plays an important role in computational linguistics, because information about language has a natural hierarchical structure (think of the way we divide sentences into noun phrases and verb phrases, and noun phrases into determiners and nouns, and so on)

#### Proof Search_ Thực thi mã
KB1:
```prolog
f(a).
f(b).

g(a).
g(b).

h(b).

k(x) :- f(X),g(X),h(X).
```
querry:
```prolog
?- k(X)
```
Prolog trả về k(b).
Đầu tiên Prolog đọc từ trên xuống và cố gắng khớp k(X) với bất kỳ luật hay quy tắc nào.
=> k(X) khớp với luật k(X) :- f(X),g(X),h(X).
Khi tìm được giá trị khớp giá trị trong luật hoặc quy tắc, Prolog khởi tạo 1 biến tượng trung cho biến đang dược chia sẻ. Câu truy vấn ban đầu sẽ được hiểu: 

k(__ G348)

Và Prolog hiểu rẳng:

k(__ G348) :- f(__ G348), g(__ G348), h(__ G348)

Bây giờ Prolog sẽ tìm bất kỳ phần tử X nào trong KB phù hợp với các quy tắc:
    f ( _ G348) , g( _ G348), h( _ G348).
![image](https://github.com/takumi612/nnlt/assets/87805462/6dc35338-a371-42e8-90a4-fab2dc60ada6)


Tiếp tục Prolog sẽ cố gắng thỏa mãn từng điều kiện , duyệt theo từng phần tử từ trái sang phải.

Phần tử đầu tiên là f (__ G348), lúc này Prolog duyệt trong KB để tìm quy tắc hoặc luật phù hợp, và Prolog sẽ tìm ra 2 kết quả f(a) và f(b).  Prolog matching lần lượt f(__ G348) với f(a), lúc này __ G348 được gán giá trị a, áp dụng với mọi quy tắc khác với đối số __ G348 . Prolog tiếp túc với các quy tắc khác:
    g(a), h(a)
![image](https://github.com/takumi612/nnlt/assets/87805462/5e3449b2-5cb6-4394-9983-c3abaf324884)

 g(a) tương tự, đến với h(a) trong KB không tồn tại. Prolog biết rằng mình đã sai và quay lui tìm giá trị phù hợp. Quay lui lên g(a) thấy không có giá trị nào khớp nữa, tiếp tục quay lui lên f(__ G348) lúc này nó thấy có thể matching với f(b). Prolog tiếp tục gán giá trị __ G348 với b.
 Tương tự như trên và tồn tại h(b) trong KB. Prolog lưu lại kết quả và tiếp tục quay lui tìm xem còn kết quả nào khả thi nữa không.
![image](https://github.com/takumi612/nnlt/assets/87805462/f7e4920e-79c7-4d88-97e4-88b8c6e2c547)

KB2:
```prolog
loves(vincent,mia). 
loves(marcellus,mia). 
jealous(X,Y) :- loves(X,Z),loves(Y,Z).
```
Querry:;
```prolog
jealous(X,Y)
```
![image](https://github.com/takumi612/nnlt/assets/87805462/1ade7f55-4567-40df-8826-f52b333e882f)

