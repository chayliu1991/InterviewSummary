# 设计实现 vector

```
#include <iostream>
#include <memory>
#include <utility>
#include <initializer_list>

template <typename T>
class MyVector final
{
public:
	MyVector()
	{
		data_ = new T[10];
		size_ = 0;
		capacity_ = 10;
	}

	explicit MyVector(size_t n)
	{
		data_ = new T[n];
		size_ = n;
		capacity_ = n;
	}

	MyVector(const std::initializer_list<T>& il)
	{
		size_t n = il.size();
		MyVector tmp(n);
		size_t i = 0;
		for (auto it = il.begin(); it != il.end(); it++)
		{
			tmp.data_[i++] = *it;
		}
		T* p = data_;
		data_ = tmp.data_;
		capacity_ = tmp.capacity_;
		size_ = tmp.size_;
		tmp.data_ = p;
	}


	MyVector(const MyVector& v)
	{
		capacity_ = v.capacity_;
		size_ = v.size_;
		data_ = new T[capacity_];
		for (size_t i = 0; i < v.size_; i++)
			data_[i] = v.data_[i];
	}

	MyVector& operator = (const MyVector& v)
	{
		if (&v != this)
		{
			MyVector tmp;
			tmp.capacity_ = v.capacity_;
			tmp.size_ = v.size_;
			tmp.data_ = new T[capacity_];
			for (size_t i = 0; i < v.size_; i++)
				tmp.data_[i] = v.data_[i];

			T* p = data_;
			data_ = tmp.data_;
			capacity_ = tmp.capacity_;
			size_ = tmp.size_;
			tmp.data_ = p;
		}
		return *this;
	}

	MyVector(MyVector&& v) noexcept
	{
		data_ = v.data_;
		size_ = v.size_;
		capacity_ = v.capacity_;

		v.data_ = nullptr;
		v.size_ = 0;
		v.capacity_ = 0;
	}

	MyVector& operator= (MyVector&& v)
	{
		MyVector tmp(v);
		T* p = data_;
		data_ = tmp.data_;
		size_ = tmp.size_;
		capacity_ = tmp.capacity_;
		tmp.data_ = p;
		return *this;
	}

	~MyVector() noexcept
	{
		delete[] data_;
		data_ = nullptr;
	}

	T& operator[](size_t index) 
	{
		return data_[index];
	}

	void reserve(size_t n)
	{
		if (n > capacity_)
		{
			data_ = new T[n];
			capacity_ = n;
		}
	}

	void resize(size_t n)
	{
		T* tmp = new T[n];
		size_t i = 0, j = 0;
		while (i < n && j < size_)
		{
			tmp[i++] = data_[j++];
		}

		delete[] data_;
		data_ = tmp;
		capacity_ = n;
		size_ = n;
	}

	void push_back(T t)
	{
		if (size_ == capacity_)
		{
			resize(capacity_ * 2);
		}

		data_[size_] = t;
		size_ += 1;
	}

	void pop_back() 
	{
		size_ -= 1;
	}

	T& back() 
	{
		return data_[size_ - 1];
	}

	T& front()
	{
		return data_[0];
	}

	T* data() const noexcept
	{
		return data_;
	}

	size_t size() const noexcept
	{
		return size_;
	}

	size_t capacity() const noexcept
	{
		return capacity_;
	}

	bool empty() const noexcept
	{
		return size_ == 0;
	}

	void clear() noexcept
	{
		size_ = 0;
	}

	template <typename F>
	friend std::ostream& operator<<(std::ostream& os, const MyVector<F>& v);

private:
	T* data_;
	size_t size_;
	size_t capacity_;
};

template<typename T>
std::ostream& operator<<(std::ostream& os, const MyVector<T>& v)
{
	for (size_t i = 0; i < v.size_; i++)
		os << v.data_[i] << " ";
	return os;
}

```

# 设计实现 string

```
#include <cstring>
#include <memory>
#include <utility>
#include <iostream>

class MyString final
{
public:
	MyString()
	{
		data_ = new char[1];
		data_[0] = '\0';
	}

	MyString(const char* str)
	{
		len_ = strlen(str);
		data_ = new char[len_ + 1];
		strcpy(data_, str);
		data_[len_] = '\0';
	}

	MyString(const MyString& str)
	{
		len_ = strlen(str.data_);
		data_ = new char[len_ + 1];
		strcpy(data_, str.data_);
		data_[len_] = '\0';
	}

	MyString& operator = (const MyString& str)
	{
		if (&str != this)
		{
			MyString tmp(str);
			char* p = data_;
			data_ = tmp.data_;
			len_ = tmp.len_;
			tmp.data_ = p;
		}
		return *this;
	}

	MyString(MyString&& str) noexcept
	{
		data_ = str.data_;
		len_ = str.len_;

		str.data_ = nullptr;
		str.len_ = 0;
	}

	MyString& operator = (MyString&& str)
	{
		MyString tmp(str);
		char* p = data_;
		data_ = tmp.data_;
		len_ = tmp.len_;
		tmp.data_ = p;
	}

	~MyString() noexcept
	{
		delete[] data_;
		data_ = nullptr;
	}

	size_t length() const noexcept
	{
		return len_;
	}

	size_t size() const noexcept
	{
		return len_;
	}

	bool empty() const noexcept
	{
		return len_ == 0;
	}

	friend std::ostream& operator << (std::ostream& os, const MyString& str);

private:
	char* data_;
	size_t len_;
};

std::ostream& operator << (std::ostream& os, const MyString& str)
{
	std::cout << str.data_;
	return os;
}
```

# 设计实现 shared_ptr

```
#include <atomic>
#include <cstdint>
#include <iostream>
#include <utility>

struct RefCount final
{
	std::atomic<long> count_{ 1 };

	void add_ref_count()noexcept
	{
		count_++;
	}

	long reduce_ref_count() noexcept
	{
		count_--;
		return count_;
	}

	long get_ref_count() const noexcept
	{
		return count_;
	}
};

template <typename T>
class SharedPtr final
{
public:
	template <typename U>
	friend class SharedPtr;

	SharedPtr(T* ptr = nullptr) : ptr_(ptr)
	{
		if (ptr_)
		{
			ref_count_ = new RefCount();
		}
	}

	~SharedPtr() noexcept
	{
		if (ptr_ && ref_count_->reduce_ref_count() == 0)
		{
			delete ptr_;
			ptr_ = nullptr;
			delete ref_count_;
			ref_count_ = nullptr;
		}
	}

	SharedPtr(const SharedPtr& other) noexcept
	{
		ptr_ = other.ptr_;
		if (ptr_)
		{
			other.ref_count_->add_ref_count();
			ref_count_ = other.ref_count_;
		}
	}

	void swap(SharedPtr& other) noexcept
	{
		std::swap(other.ptr_, ptr_);
		std::swap(other.ref_count_, ref_count_);
	}

	SharedPtr& operator = (SharedPtr other) noexcept
	{
		other.swap(*this);
		return *this;
	}

	template <typename U>
	SharedPtr(const SharedPtr<U>& other) noexcept
	{
		ptr_ = other.ptr_;
		if (ptr_)
		{
			other.ref_count_->add_ref_count();
			ref_count_ = other->ref_count_;
		}
	}

	template <typename U>
	SharedPtr(const SharedPtr<U>& other,T* ptr) noexcept
	{
		ptr_ = other.ptr;
		if (ptr_)
		{
			other.ref_count_->add_ref_count();
			ref_count_ = other->ref_count_;
		}
	}


	template <typename U>
	SharedPtr(SharedPtr<U>&& other) noexcept
	{
		ptr_ = other.ptr_;
		if (ptr_)
		{
			ref_count_ = other.ref_count_;
			other.ptr_ = nullptr;
		}
	}

	T* get() const noexcept
	{
		return ptr_;
	}

	T& operator*() const noexcept
	{
		return *ptr_;
	}

	T* operator->() const noexcept
	{
		return ptr_;
	}

	long use_count() const noexcept
	{
		if (ptr_)
			return ref_count_->get_ref_count();
		return 0;
	}

	operator bool() const noexcept
	{
		return ptr_ != nullptr;
	}

private:
	T* ptr_;
	RefCount* ref_count_;
};


template <typename T, typename U>
SharedPtr<T> static_cast_ptr(SharedPtr<U> other)
{
	T* ptr = static_cast<T*>(other.get());
	return SharedPtr<T>(other, ptr);
}

template <typename T, typename U>
SharedPtr<T> reinterpret_cast_ptr(const SharedPtr<U>& other) noexcept
{
	T* ptr = reinterpret_cast<T*> (other.get());
	return SharedPtr<T>(other, ptr);
}

template <typename T, typename U>
SharedPtr<T> const_cast_ptr(const SharedPtr<U>& other) noexcept
{
	T* ptr = const_cast<T*> (other.get());
	return SharedPtr<T>(other, ptr);
}

template <typename T, typename U>
SharedPtr<T> dynamic_cast_ptr(const SharedPtr<U>& other) noexcept
{
	T* ptr = dynamic_cast<T*> (other.get());
	return SharedPtr<T>(other, ptr);
}

template <typename T, typename ...Args>
SharedPtr<T> make_shared_ptr(Args...args)
{
	return SharedPtr<T>(new T(std::forward<Args>(args)...));
}
```

# 设计实现 blocked_queue

```
#include <thread>
#include <list>
#include <mutex>
#include <condition_variable>

template <typename T>
class BlockedQueue final
{
public:
	explicit BlockedQueue(size_t cap) : capacity_(cap)
	{
	}

	void push(T&& t)
	{
		add(t);
	}

	void push(const T & t)
	{
		add(t);
	}

	void pop(std::list<T> & list)
	{
		std::unique_lock<std::mutex> lock(mtx_);
		cv_not_empty_.wait(lock, [this] {return need_stop_ || !is_empty(); });
		if (need_stop_)
			return;
		list = std::move(data_);
		cv_not_full_.notify_one();
	}

	void front(T& t)
	{
		std::unique_lock<std::mutex> lock(mtx_);
		cv_not_empty_.wait(lock, [this] {return need_stop_ || !is_empty(); });
		if (need_stop_)
			return;

		t = std::move(data_.front());
		data_.pop_front();
		cv_not_full_.notify_one();
	}

	bool empty() const noexcept
	{
		std::lock_guard<std::mutex> lock(mtx_);
		return is_empty();
	}

	bool full() const noexcept
	{
		std::lock_guard<std::mutex> lock(mtx_);
		return is_full();
	}

	void size() const noexcept
	{
		std::lock_guard<std::mutex> lock(mtx_);
		return data_.size();
	}
	
	void stop()
	{
		{
			std::lock_guard<std::mutex> lock(mtx_);
			need_stop_ = true;
		}

		cv_not_empty_.notify_all();
		cv_not_full_.notify_all();
	}

private: 
	template <typename F>
	void add(F&& f)
	{
		std::unique_lock<std::mutex> lock(mtx_);
		cv_not_full_.wait(lock, [this] {return need_stop_ || !is_full(); });
		if (need_stop_)
			return;

		data_.push_back(std::forward<F>(f));
		cv_not_empty_.notify_one();
	}

	bool is_full() const noexcept
	{
		return data_.size() >= capacity_;
	}

	bool is_empty() const noexcept
	{
		return data_.empty();
	}

	bool need_stop_{false};
	std::list<T> data_;
	std::mutex mtx_;
	std::condition_variable cv_not_full_, cv_not_empty_;
	size_t capacity_{0};
};
```

# 设计实现线程池

```
#include <list>
#include <thread>
#include <functional>
#include <atomic>
#include <algorithm>

const int MaxTaskNums = 20;
class ThreadPool final
{
public:
	using Task = std::function<void()>;

	ThreadPool(size_t nums = std::thread::hardware_concurrency()) : running_(false),task_queue_(MaxTaskNums)
	{
		start(nums);
	}

	~ThreadPool()
	{
		stop();
	}

	void stop()
	{
		std::call_once(flag_, [this] {stop_all_threads(); });
	}

	void add_task(Task&& task)
	{
		task_queue_.push(std::forward<Task>(task));
	}

	void add_task(const Task& task)
	{
		task_queue_.push(task);
	}

private:
	void start(size_t n)
	{
		running_.store(true);
		for (size_t i = 0; i < n; i++)
		{
			auto ptr = std::make_shared<std::thread>(&ThreadPool::run_in_thread,this);
			threads_.emplace_back(ptr);
		}
	}

	void stop_all_threads()
	{
		task_queue_.stop();
		running_ = false;

		std::for_each(threads_.begin(), threads_.end(),std::mem_fn(&std::thread::join));
		threads_.clear();
	}

	void run_in_thread()
	{
		while (running_)
		{
			std::list<Task> tasks;
			task_queue_.pop(tasks);
			{
				for (const auto& task : tasks)
				{
					if (!running_)
						return;
					task();
				}
			}
		}
	}

private:
	std::once_flag flag_;
	std::atomic<bool> running_;
	BlockedQueue<Task> task_queue_;
	std::list<std::shared_ptr<std::thread>> threads_;
};
```

# 设计多生成者多消费者

```
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>

static const int kItemNums = 10;
static const int kItemToProduce = 100000;

struct ItemRepo
{
	int buffer[kItemNums];
	size_t consume_pos = 0;
	size_t produce_pos = 0;
	size_t consume_cnt = 0;
	size_t produce_cnt = 0;
	std::mutex mtx;
	std::mutex produce_cnt_mtx;
	std::mutex consume_cnt_mtx;
	std::condition_variable cv_not_full;
	std::condition_variable cv_not_empty;
};

ItemRepo g_item_repo;


void produce_task()
{
	while (true)
	{
		std::lock_guard<std::mutex> produce_lock(g_item_repo.produce_cnt_mtx);
		if (g_item_repo.produce_cnt < kItemToProduce)
		{
			std::unique_lock<std::mutex> not_full_lock(g_item_repo.mtx);
			g_item_repo.cv_not_full.wait(not_full_lock, []() {
				return (g_item_repo.produce_pos + 1)% kItemNums != g_item_repo.consume_pos;
			});

			g_item_repo.produce_cnt += 1;
			int data = g_item_repo.produce_cnt;
			g_item_repo.buffer[g_item_repo.produce_pos] = data;
			g_item_repo.produce_pos++;
			
			if (g_item_repo.produce_pos == kItemNums)
				g_item_repo.produce_pos = 0;
			
			std::cout << std::this_thread::get_id() << " produce the: " << data << " item" << std::endl;
			g_item_repo.cv_not_empty.notify_one();
		}
		else
			break;
	}
}

void consume_task()
{
	while (true)
	{
		std::lock_guard<std::mutex> consume_lock(g_item_repo.consume_cnt_mtx);
		if (g_item_repo.consume_cnt < kItemToProduce)
		{
			std::unique_lock<std::mutex> not_empty_lock(g_item_repo.mtx);
			g_item_repo.cv_not_empty.wait(not_empty_lock, []() {
				return g_item_repo.consume_pos != g_item_repo.produce_pos;
			});

			int data = g_item_repo.buffer[g_item_repo.consume_pos];
			g_item_repo.consume_pos++;
			if (g_item_repo.consume_pos == kItemNums)
				g_item_repo.consume_pos = 0;

			g_item_repo.consume_cnt += 1;
			std::cout << std::this_thread::get_id() << " consume the: " << data << " item" << std::endl;
			g_item_repo.cv_not_full.notify_one();
		}
		else
			break;
	}
}

int main()
{
	std::thread produce_thr1(produce_task);
	std::thread produce_thr2(produce_task);
	std::thread consume_thr1(consume_task);
	std::thread consume_th2(consume_task);

	produce_thr1.join();
	produce_thr2.join();
	consume_thr1.join();
	consume_th2.join();

	return 0;
}
```

