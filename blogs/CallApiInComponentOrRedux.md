# Nên call API trong component hay call trong redux

* Nên call trong redux vì store của redux độc lập với các state
* Nếu call trong component sẽ có case component unmount nhưng request không được cancel