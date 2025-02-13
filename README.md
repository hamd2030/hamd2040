
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>نظام إدارة العملاء</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
  <style>
    /* جميع الأنماط السابقة مع إضافة تحسينات */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'Arial', sans-serif;
    }

    body {
      background: linear-gradient(135deg, #f8f9fa, #e9ecef);
      min-height: 100vh;
    }

    .container {
      max-width: 1200px;
      margin: 0 auto;
      padding: 20px;
    }

    /* تحسينات واجهة تسجيل الدخول */
    .login-card {
      max-width: 500px;
      margin: 50px auto;
      background: #fff;
      border-radius: 20px;
      box-shadow: 0 10px 30px rgba(0,0,0,0.1);
      padding: 40px;
    }

    .login-card h2 {
      text-align: center;
      color: #2c3e50;
      margin-bottom: 30px;
      font-size: 2rem;
    }

    .login-form input {
      width: 100%;
      padding: 15px;
      margin: 15px 0;
      border: 2px solid #e0e0e0;
      border-radius: 10px;
      font-size: 1.1rem;
      transition: all 0.3s ease;
    }

    .login-form input:focus {
      border-color: #3498db;
      box-shadow: 0 0 8px rgba(52,152,219,0.2);
      outline: none;
    }

    /* تحسينات الأزرار */
    .btn {
      width: 100%;
      padding: 15px;
      font-size: 1.1rem;
      border-radius: 10px;
      margin-top: 20px;
      transition: transform 0.3s, box-shadow 0.3s;
    }

    .btn:hover {
      transform: translateY(-2px);
      box-shadow: 0 5px 15px rgba(0,0,0,0.2);
    }

    /* تحسينات النوافذ المنبثقة */
    .modal-content {
      background: #fff;
      padding: 30px;
      border-radius: 15px;
      box-shadow: 0 10px 40px rgba(0,0,0,0.2);
    }

    /* إضافة مؤشر التحميل */
    .loader {
      border: 4px solid #f3f3f3;
      border-top: 4px solid #3498db;
      border-radius: 50%;
      width: 40px;
      height: 40px;
      animation: spin 1s linear infinite;
      margin: 20px auto;
      display: none;
    }

    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }

    /* بقية الأنماط كما هي مع تعديلات بسيطة */
    /* ... */
  </style>
</head>
<body>

<!-- واجهة تسجيل الدخول -->
<div id="login-section" class="container">
  <div class="card login-card">
    <h2><i class="fas fa-sign-in-alt"></i> تسجيل الدخول</h2>
    <form id="login-form" class="login-form">
      <input type="text" id="login-username" placeholder="اسم المستخدم" required>
      <input type="password" id="login-password" placeholder="كلمة المرور" required>
      <div id="login-error" class="error"></div>
      <button type="submit" class="btn btn-upgrade">
        <span class="button-text">تسجيل الدخول</span>
        <div class="loader" id="login-loader"></div>
      </button>
    </form>
  </div>
</div>

<!-- بقية الصفحة كما هي -->
<script>
  const API_BASE_URL = "http://88.218.78.67/app";
  
  async function handleLogin(e) {
    e.preventDefault();
    const form = e.target;
    const loader = document.getElementById('login-loader');
    const errorElement = document.getElementById('login-error');
    const buttonText = form.querySelector('.button-text');

    loader.style.display = 'inline-block';
    buttonText.style.display = 'none';
    errorElement.textContent = '';

    try {
      const response = await fetch(API_BASE_URL + '/login', {
        method: "POST",
        mode: 'cors',
        headers: { 
          "Content-Type": "application/x-www-form-urlencoded",
          "adv_auth": "d2fbcf3996fe634c19b0222ee027d410",
          "System-ID": "7720",
          "Origin": window.location.origin
        },
        body: new URLSearchParams({
          username: document.getElementById('login-username').value.trim(),
          password: document.getElementById('login-password').value.trim(),
          system_id: "7720"
        })
      });

      const data = await response.json();

      if (!response.ok || data.error) {
        throw new Error(data.message || 'خطأ في المصادقة (403) - تأكد من البيانات');
      }

      localStorage.setItem('userData', JSON.stringify(data.account));
      window.location.href = '#client-section';

    } catch (error) {
      errorElement.textContent = error.message;
      console.error('Error:', error);
    } finally {
      loader.style.display = 'none';
      buttonText.style.display = 'inline-block';
    }
  }

  document.getElementById("login-form").addEventListener("submit", handleLogin);
</script>

</body>
</html>
