#Cách hoạt động reducer
* Khi có một action dispatch lên store, thì root reducer sẽ gửi tín hiệu đến các subreducer, các subreducer tạo bản state copy thay đổi trên bản copy đó rồi gửi lại cho rootreducer
* rootreducer tổng hợp các bản copies từ subreducer tạo thành 1 date state tree và gửi lại cho store
* store thay thế state tree cũ bằng state tree mới
https://viblo.asia/p/redux-cho-nguoi-moi-bat-dau-part-1-introduction-ZjleaBBZkqJ