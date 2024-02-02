# Introduction
Chúng ta sẽ bắt đầu với cái nhìn tổng quan về cách hoạt động của các mô hình học máy và cách chúng được sử dụng. Điều này có vẻ cơ bản nếu bạn đã thực hiện lập mô hình thống kê hoặc học máy trước đây. Đừng lo lắng, chúng tôi sẽ sớm tiến tới xây dựng các mô hình mạnh mẽ.

Bài toán : Anh họ của bạn đã kiếm được hàng triệu đô la nhờ đầu cơ bất động sản. Anh ấy đề nghị trở thành đối tác kinh doanh với bạn vì bạn quan tâm đến khoa học dữ liệu. Anh ta sẽ cung cấp tiền và bạn sẽ cung cấp các mô hình dự đoán giá trị của các ngôi nhà khác nhau.

Bạn hỏi anh họ của mình xem trước đây anh ấy dự đoán giá trị bất động sản như thế nào và anh ấy nói rằng đó chỉ là trực giác. Nhưng nhiều câu hỏi hơn tiết lộ rằng anh ấy đã xác định được các mô hình giá từ những ngôi nhà mà anh ấy đã thấy trước đây và anh ấy sử dụng những mô hình đó để đưa ra dự đoán cho những ngôi nhà mới mà anh ấy đang xem xét.

Học máy hoạt động theo cách tương tự. Chúng ta sẽ bắt đầu với một mô hình có tên là Cây quyết định. Có những mô hình đẹp hơn đưa ra những dự đoán chính xác hơn. Nhưng cây quyết định rất dễ hiểu và chúng là khối xây dựng cơ bản cho một số mô hình tốt nhất trong khoa học dữ liệu. 

Để đơn giản, chúng ta sẽ bắt đầu với cây quyết định đơn giản nhất có thể
![Alt Text](https://storage.googleapis.com/kaggle-media/learn/images/7tsb5b1.png)
=> Nó chia nhà thành hai loại. Giá dự đoán cho bất kỳ ngôi nhà nào đang được xem xét là giá trung bình lịch sử của những ngôi nhà cùng loại.

- Chúng sử dụng dữ liệu để quyết định cách chia các ngôi nhà thành hai nhóm và sau đó xác định lại giá dự đoán ở mỗi nhóm. Bước thu thập các mẫu từ dữ liệu này được gọi là **điều chỉnh hoặc huấn luyện** mô hình. Dữ liệu được sử dụng để **phù hợp** với mô hình được gọi là **dữ liệu huấn luyện**.

- Các chi tiết về mức độ phù hợp của mô hình (ví dụ: cách phân chia dữ liệu) đủ phức tạp để chúng tôi sẽ lưu lại sau. Sau khi mô hình đã phù hợp, bạn có thể áp dụng nó vào dữ liệu mới để **dự đoán** giá của những ngôi nhà bổ sung.
# Improving the Decision Tree
![alt text](https://storage.googleapis.com/kaggle-media/learn/images/prAjgku.png)   
Cây quyết định ở bên trái (Cây quyết định 1) có lẽ hợp lý hơn, vì nó phản ánh thực tế rằng những ngôi nhà có nhiều phòng ngủ có xu hướng bán với giá cao hơn những ngôi nhà có ít phòng ngủ. Thiếu sót lớn nhất của mô hình này là nó không nắm bắt được hầu hết các yếu tố ảnh hưởng đến giá nhà, như số lượng phòng tắm, quy mô lô đất, vị trí, v.v.

Bạn có thể nắm bắt được nhiều yếu tố hơn bằng cách sử dụng cây có nhiều "phần chia" hơn. Chúng được gọi là cây "sâu hơn". Cây quyết định cũng xem xét tổng diện tích lô đất của mỗi ngôi nhà có thể trông như thế này:
![alt text](https://storage.googleapis.com/kaggle-media/learn/images/R3ywQsR.png)
Bạn dự đoán giá của bất kỳ ngôi nhà nào bằng cách truy tìm cây quyết định, luôn chọn đường đi tương ứng với đặc điểm của ngôi nhà đó. Giá dự đoán của ngôi nhà nằm ở phần dưới cùng của cây. Điểm ở phía dưới nơi chúng ta đưa ra dự đoán được gọi là chiếc **lá**.
# Xác thực mô hình là gì
Bạn sẽ muốn đánh giá hầu hết mọi mô hình bạn từng xây dựng. Trong hầu hết (mặc dù không phải tất cả) ứng dụng, thước đo liên quan đến chất lượng mô hình là độ chính xác dự đoán. Nói cách khác, liệu những dự đoán của mô hình có gần với những gì thực sự xảy ra hay không.

Nhiều người mắc sai lầm lớn khi đo lường độ chính xác của dự đoán. Họ đưa ra dự đoán bằng dữ liệu huấn luyện của mình và so sánh những dự đoán đó với giá trị mục tiêu trong dữ liệu huấn luyện. Bạn sẽ thấy vấn đề với cách tiếp cận này và cách giải quyết nó ngay lập tức, nhưng trước tiên hãy nghĩ về cách chúng ta sẽ làm điều này

Trước tiên, bạn cần tóm tắt chất lượng mô hình một cách dễ hiểu. Nếu bạn so sánh giá trị ngôi nhà được dự đoán và thực tế của 10.000 ngôi nhà, bạn có thể sẽ tìm thấy những dự đoán tốt và xấu đan xen. Nhìn qua danh sách 10.000 giá trị dự đoán và thực tế sẽ là vô nghĩa. Chúng ta cần tóm tắt điều này thành một số liệu duy nhất.

Có nhiều số liệu để tóm tắt chất lượng mô hình, nhưng chúng ta sẽ bắt đầu với một số liệu gọi là Lỗi tuyệt đối trung bình (còn gọi là **MAE**). Hãy chia nhỏ số liệu này bắt đầu bằng từ cuối cùng, lỗi.

Với chỉ số MAE, chúng tôi lấy giá trị tuyệt đối của từng lỗi. Điều này chuyển đổi mỗi lỗi thành một số dương. Sau đó chúng tôi lấy mức trung bình của những lỗi tuyệt đối đó. Đây là thước đo chất lượng mô hình của chúng tôi

Để tính MAE, trước tiên chúng ta cần một mô hình. Điều đó được tích hợp trong một ô ẩn bên dưới băng thu viện của sklearn.
Link data: <https://www.kaggle.com/datasets/dansbecker/melbourne-housing-snapshot>
```python
# Data Loading Code Hidden Here
import pandas as pd

# Load data
melbourne_file_path = '../input/melbourne-housing-snapshot/melb_data.csv'
melbourne_data = pd.read_csv(melbourne_file_path) 
# Filter rows with missing price values
filtered_melbourne_data = melbourne_data.dropna(axis=0)
# Choose target and features
y = filtered_melbourne_data.Price
melbourne_features = ['Rooms', 'Bathroom', 'Landsize', 'BuildingArea', 
                        'YearBuilt', 'Lattitude', 'Longtitude']
X = filtered_melbourne_data[melbourne_features]
```
```python
from sklearn.tree import DecisionTreeRegressor
# Define model
melbourne_model = DecisionTreeRegressor()
# Fit model
melbourne_model.fit(X, y)
from sklearn.metrics import mean_absolute_error

predicted_home_prices = melbourne_model.predict(X)
mean_absolute_error(y, predicted_home_prices)
```
out_put: 434.71594577146544
# Underfitting and Overfitting
![alt text](https://storage.googleapis.com/kaggle-media/learn/images/R3ywQsR.png)
Trong thực tế, không có gì lạ khi một cái cây có 10 nhánh giữa ngọn (tất cả các ngôi nhà) và một chiếc lá. Khi cây càng sâu, tập dữ liệu sẽ được chia thành các lá với ít ngôi nhà hơn. Nếu một cây chỉ có 1 phần chia thì nó sẽ chia dữ liệu thành 2 nhóm. Nếu chia mỗi nhóm lại thì chúng ta sẽ có 4 nhóm nhà. Tách từng nhóm một lần nữa sẽ tạo ra 8 nhóm. Nếu chúng ta tiếp tục tăng gấp đôi số nhóm bằng cách thêm nhiều phần chia ở mỗi cấp, chúng ta sẽ có $2^{10}$ các nhóm nhà khi chúng tôi lên đến tầng 10. Đó là 1024 lá.

Khi chia các ngôi nhà ra nhiều lá, chúng ta cũng có ít nhà hơn trong mỗi lá. Những lá có rất ít ngôi nhà sẽ đưa ra những dự đoán khá gần với giá trị thực tế của những ngôi nhà đó, nhưng có thể lại đưa ra những dự đoán rất không đáng tin cậy đối với dữ liệu mới (vì mỗi dự đoán chỉ dựa trên một vài ngôi nhà).=> Đây là một hiện tượng được gọi là **Overfitting (trang bị quá mức)**, trong đó một mô hình khớp gần như hoàn hảo với dữ liệu huấn luyện nhưng lại kém hiệu quả trong việc xác thực và các dữ liệu mới khác. Mặt khác, nếu chúng ta làm cho cây rất nông, nó sẽ không chia các ngôi nhà thành các nhóm rất riêng biệt.

Ở mức độ cực đoan, nếu một cái cây chỉ chia các ngôi nhà thành 2 hoặc 4 thì mỗi nhóm vẫn có rất nhiều loại nhà. Các dự đoán kết quả có thể khác xa đối với hầu hết các ngôi nhà, ngay cả trong dữ liệu huấn luyện (và nó cũng sẽ rất tệ trong việc xác thực vì lý do tương tự). Khi một mô hình không nắm bắt được các điểm khác biệt và mẫu quan trọng trong dữ liệu, do đó nó hoạt động kém ngay cả trong dữ liệu huấn luyện, điều đó được gọi là **underfitting (không phù hợp)**.

Vì chúng tôt quan tâm đến độ chính xác của dữ liệu mới mà chúng ta ước tính từ dữ liệu xác thực của mình, nên chúng ta muốn tìm ra điểm phù hợp giữa trang bị thiếu và trang bị quá mức. Nhìn trực quan, chúng tôi muốn điểm thấp của đường cong xác thực (màu đỏ) trong hình bên dưới. 
![alt text](https://storage.googleapis.com/kaggle-media/learn/images/AXSEOfI.png)
Chúng ta có thể sử dụng hàm tiện ích để giúp so sánh điểm MAE từ các giá trị khác nhau cho max_leaf_nodes:
```python
from sklearn.metrics import mean_absolute_error
from sklearn.tree import DecisionTreeRegressor

def get_mae(max_leaf_nodes, train_X, val_X, train_y, val_y):
    model = DecisionTreeRegressor(max_leaf_nodes=max_leaf_nodes, random_state=0)
    model.fit(train_X, train_y)
    preds_val = model.predict(val_X)
    mae = mean_absolute_error(val_y, preds_val)
    return(mae)
```
Chúng ta có thể sử dụng vòng lặp for để so sánh độ chính xác của các mô hình được xây dựng với các giá trị khác nhau cho max_leaf_nodes.
```python
# compare MAE with differing values of max_leaf_nodes
for max_leaf_nodes in [5, 50, 500, 5000]:
    my_mae = get_mae(max_leaf_nodes, train_X, val_X, train_y, val_y)
    print("Max leaf nodes: %d  \t\t Mean Absolute Error:  %d" %(max_leaf_nodes, my_mae))
```
- Trong số các phương án được liệt kê, 500 là số lá tối ưu.
   ### Conclusion
    Đây là bài học rút ra: Người mẫu có thể mắc phải một trong hai trường hợp sau:

    - Trang bị quá mức: nắm bắt các mẫu giả sẽ không tái diễn trong tương lai, dẫn đến dự đoán kém chính xác hơn hoặc
    - Trang bị chưa đầy đủ: không nắm bắt được các mẫu có liên quan, một lần nữa dẫn đến dự đoán kém chính xác hơn.
    
    Chúng ta sử dụng dữ liệu xác thực không được sử dụng trong đào tạo mô hình để đo lường độ chính xác của mô hình ứng viên. Điều này cho phép chúng tôi thử nhiều mô hình ứng cử viên và giữ lại mô hình tốt nhất.
# Random Forests
**Cây quyết định** để lại cho bạn một quyết định khó khăn. Một cái cây sâu có nhiều lá sẽ quá phù hợp vì mỗi dự đoán đều đến từ dữ liệu lịch sử chỉ từ một vài ngôi nhà trên lá của nó. Nhưng một cây nông có ít lá sẽ hoạt động kém vì nó không thể hiện được nhiều điểm khác biệt trong dữ liệu thô.

Ngay cả những kỹ thuật lập mô hình phức tạp nhất hiện nay cũng phải đối mặt với sự căng thẳng giữa trang bị thiếu và trang bị quá mức. Tuy nhiên, nhiều mô hình có những ý tưởng thông minh có thể mang lại hiệu suất tốt hơn. Chúng ta sẽ lấy khu rừng ngẫu nhiên làm ví dụ.

**Random Forests** sử dụng nhiều cây và đưa ra dự đoán bằng cách lấy trung bình các dự đoán của từng cây thành phần. Nhìn chung, nó có độ chính xác dự đoán tốt hơn nhiều so với cây quyết định đơn lẻ và hoạt động tốt với các tham số mặc định. Nếu tiếp tục lập mô hình, bạn có thể tìm hiểu thêm các mô hình với hiệu suất thậm chí còn tốt hơn, nhưng nhiều mô hình trong số đó rất nhạy cảm với việc nhận được thông số phù hợp.
- Chúng ta xây dựng mô hình rừng ngẫu nhiên tương tự như cách chúng tôi xây dựng cây quyết định trong scikit-learn - lần này sử dụng lớp RandomForestRegressor thay vì DecisionTreeRegressor.
```python
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_absolute_error

forest_model = RandomForestRegressor(random_state=1)
forest_model.fit(train_X, train_y)
melb_preds = forest_model.predict(val_X)
print(mean_absolute_error(val_y, melb_preds))
```
