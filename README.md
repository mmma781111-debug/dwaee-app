<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>منظومة دوائي Dwaee - لوحة الصيدلاني</title>
    
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="theme-color" content="#0f172a">

    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        :root {
            --bg-color: #f1f5f9;
            --sidebar-bg: #0f172a;
            --primary-color: #3b82f6;
            --success-color: #10b981;
            --warning-color: #f59e0b;
            --danger-color: #ef4444;
            --card-bg: #ffffff;
            --text-main: #1e293b;
            --text-muted: #64748b;
        }

        body {
            font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background-color: var(--bg-color);
            margin: 0; padding: 0;
            color: var(--text-main);
            overflow-x: hidden;
            height: 100vh;
            display: flex;
            flex-direction: column;
            overscroll-behavior-y: contain; /* منع السحب العشوائي للمتصفح */
        }

        /* حاوية محتوية ممركزة ومثالية للموبايل */
        .app-container {
            width: 100%;
            max-width: 500px; /* حجم مثالي جداً ليوجه التطبيق بنص الشاشة */
            margin: 0 auto;
            background: var(--bg-color);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            box-sizing: border-box;
            padding: 12px;
            gap: 12px;
        }

        .app-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: var(--sidebar-bg);
            color: white;
            padding: 15px;
            border-radius: 12px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.05);
        }

        .brand-logo {
            font-weight: 800;
            font-size: 18px;
            color: white;
            text-decoration: none;
        }
        .brand-logo span { color: var(--success-color); }

        .status-badge {
            background: rgba(16, 185, 129, 0.2);
            color: var(--success-color);
            padding: 4px 10px;
            border-radius: 20px;
            font-size: 11px;
            font-weight: bold;
        }

        /* كارد الروشتة المستلمة */
        .rx-card {
            background: var(--card-bg);
            border-radius: 12px;
            padding: 16px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.02);
            border-top: 4px solid var(--primary-color);
        }

        .patient-info-title {
            font-size: 15px;
            font-weight: bold;
            margin-bottom: 10px;
            color: var(--sidebar-bg);
            display: flex;
            align-items: center;
            gap: 6px;
        }

        /* قائمة العلاجات داخل الروشتة */
        .med-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 8px;
            border-bottom: 1px solid #f1f5f9;
            font-size: 14px;
        }
        .med-item:last-child { border-bottom: none; }
        
        .med-name { font-weight: 600; }
        .med-status-icon { font-size: 16px; color: #cbd5e1; }
        .med-status-icon.scanned { color: var(--success-color); }

        /* منطقة كاميرا الباركود والمخزن الفوري */
        .barcode-section {
            background: var(--card-bg);
            border-radius: 12px;
            padding: 15px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.02);
        }

        .section-title {
            font-size: 14px;
            font-weight: bold;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            gap: 6px;
            color: var(--text-muted);
        }

        .camera-simulation {
            width: 100%;
            height: 140px;
            background: #000;
            border-radius: 10px;
            position: relative;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            color: white;
            overflow: hidden;
            margin-bottom: 10px;
        }
        .laser-line {
            position: absolute;
            width: 90%; height: 2px;
            background: var(--danger-color);
            top: 50%; left: 5%;
            box-shadow: 0 0 8px var(--danger-color);
            animation: scanAnimation 2s infinite ease-in-out;
        }

        @keyframes scanAnimation {
            0% { top: 20%; }
            50% { top: 80%; }
            100% { top: 20%; }
        }

        .btn-action {
            width: 100%;
            padding: 12px;
            border: none;
            border-radius: 8px;
            font-weight: bold;
            font-size: 14px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            box-sizing: border-box;
        }
        .btn-scan { background: var(--sidebar-bg); color: white; margin-bottom: 8px; }
        .btn-finish { background: var(--success-color); color: white; }

        /* المخزن السريع */
        .stock-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 8px;
        }
        .stock-box {
            background: #f8fafc;
            border: 1px solid #e2e8f0;
            padding: 10px;
            border-radius: 8px;
            font-size: 12px;
            text-align: center;
        }
        .stock-count {
            font-size: 16px;
            font-weight: bold;
            color: var(--primary-color);
            margin-top: 4px;
        }
    </style>
</head>
<body>

    <div class="app-container">
        
        <div class="app-header">
            <a href="#" class="brand-logo">دوائي<span>.الصيدلية</span></a>
            <div class="status-badge" id="netStatus"><i class="fa-solid fa-circle-dot"></i> متصل لايف</div>
        </div>

        <div class="rx-card">
            <div class="patient-info-title">
                <i class="fa-solid fa-receipt" style="color:var(--primary-color)"></i>
                <span>الروشتة المستلمة:</span>
                <span id="livePatientName" style="color:var(--primary-color); margin-right:auto;">بانتظار طبيب...</span>
            </div>
            
            <div id="liveMedsList" style="margin-top: 10px;">
                <p style="color: var(--text-muted); text-align: center; font-size: 13px;">لا توجد روشتات نشطة حالياً. أرسل واحدة من لوحة الدكتور</p>
            </div>
        </div>

        <div class="barcode-section">
            <div class="section-title"><i class="fa-solid fa-camera"></i> قارئ الباركود الذكي (محاكاة الكاميرا)</div>
            <div class="camera-simulation">
                <div class="laser-line"></div>
                <i class="fa-solid fa-qrcode" style="font-size: 40px; opacity: 0.3; margin-bottom: 8px;"></i>
                <span style="font-size: 12px; opacity: 0.8;" id="cameraStatus">توجيه الكاميرا نحو العلاج...</span>
            </div>
            <button class="btn-action btn-scan" id="btnTriggerScan"><i class="fa-solid fa-expand"></i> قراءة باركود سريع (تجربة)</button>
        </div>

        <div class="barcode-section">
            <div class="section-title"><i class="fa-solid fa-boxes-stacked"></i> مخزن الصيدلية السريع</div>
            <div class="stock-grid">
                <div class="stock-box">
                    <strong>Amoxicillin (111)</strong>
                    <div class="stock-count" id="stock-111">48 علبة</div>
                </div>
                <div class="stock-box">
                    <strong>Paracetamol (222)</strong>
                    <div class="stock-count" id="stock-222">120 علبة</div>
                </div>
            </div>
            <button class="btn-action btn-finish" id="btnDeliverRx" style="margin-top: 12px;" disabled><i class="fa-solid fa-check-double"></i> صرف الروشتة وإغلاق الملف</button>
        </div>

    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, collection, query, where, onSnapshot, doc, updateDoc } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = {
          apiKey: "AIzaSyC_k69WKhgdc_ocykxewWP2Mnr8u_780yU",
          authDomain: "clinc-app-854a5.firebaseapp.com",
          projectId: "clinc-app-854a5",
          storageBucket: "clinc-app-854a5.firebasestorage.app",
          messagingSenderId: "245805135578",
          appId: "1:245805135578:web:99ee6cb9a24cae4eb6d7e3"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let currentRxId = null;
        let currentMeds = [];

        const q = query(collection(db, "prescriptions"), where("status", "==", "pending"));
        
        onSnapshot(q, (snapshot) => {
            const listDiv = document.getElementById("liveMedsList");
            const nameSpan = document.getElementById("livePatientName");
            
            if (snapshot.empty) {
                nameSpan.innerText = "بانتظار طبيب...";
                listDiv.innerHTML = `<p style="color: var(--text-muted); text-align: center; font-size: 13px;">لا توجد روشتات نشطة حالياً</p>`;
                currentRxId = null;
                document.getElementById("btnDeliverRx").disabled = true;
                return;
            }

            snapshot.forEach((docSnap) => {
                currentRxId = docSnap.id;
                const data = docSnap.data();
                nameSpan.innerText = data.name;
                currentMeds = data.meds || [];
                
                let html = "";
                currentMeds.forEach((m, idx) => {
                    let icon = m.scanned ? '<i class="fa-solid fa-circle-check med-status-icon scanned"></i>' : '<i class="fa-regular fa-circle med-status-icon"></i>';
                    let bg = m.scanned ? 'style="background: #f0fdf4;"' : '';
                    html += `
                        <div class="med-item" ${bg}>
                            <span class="med-name">${m.name} <small style="color:var(--text-muted); font-weight:normal;">(${m.barcode})</small></span>
                            ${icon}
                        </div>
                    `;
                });
                listDiv.innerHTML = html;
                checkIfAllScanned();
            });
        });

        function checkIfAllScanned() {
            if(currentMeds.length > 0 && currentMeds.every(m => m.scanned)) {
                document.getElementById("btnDeliverRx").disabled = false;
            } else {
                document.getElementById("btnDeliverRx").disabled = true;
            }
        }

        // محاكاة قراءة الباركود عن طريق الكاميرا
        document.getElementById("btnTriggerScan").addEventListener("click", async () => {
            if(!currentRxId) { alert("🚨 لا توجد روشتة نشطة لقراءتها!"); return; }
            
            let unscanned = currentMeds.find(m => !m.scanned);
            if(!unscanned) { alert("🎉 كل الأدوية ممسوحة مسبقاً!"); return; }

            const targetBarcode = unscanned.barcode;
            document.getElementById("cameraStatus").innerText = `✅ تم التقاط الباركود: ${targetBarcode}`;
            
            // اهتزاز الهاتف عند القراءة الناجحة لمزيد من الواقعية
            if (navigator.vibrate) { navigator.vibrate(150); }

            currentMeds = currentMeds.map(m => {
                if(m.barcode === targetBarcode) return { ...m, scanned: true };
                return m;
            });

            // تقليل كمية المخزن بمقدار علبة واحدة مجازياً
            let stockEl = document.getElementById(`stock-${targetBarcode}`);
            if(stockEl) {
                let currentStock = parseInt(stockEl.innerText);
                if(currentStock > 0) stockEl.innerText = `${currentStock - 1} علبة`;
            }

            await updateDoc(doc(db, "prescriptions", currentRxId), { meds: currentMeds });
            document.getElementById("cameraStatus").innerText = "توجيه الكاميرا نحو العلاج...";
        });

        document.getElementById("btnDeliverRx").addEventListener("click", async () => {
            if(!currentRxId) return;
            await updateDoc(doc(db, "prescriptions", currentRxId), { status: "completed" });
            alert("📦 تم تسليم الأدوية للمريض بنجاح، وتحديث المخزن فورا!");
        });
    </script>
</body>
</html>
