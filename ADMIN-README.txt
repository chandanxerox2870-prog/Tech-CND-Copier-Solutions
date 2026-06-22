<!DOCTYPE html>
<html lang="bn">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Tech CND Admin Panel</title>
  <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />

  <!-- PWA App Install -->
  <link rel="manifest" href="manifest.json">
  <meta name="theme-color" content="#ce1212">
  <meta name="mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-title" content="Tech CND">
  <link rel="apple-touch-icon" href="icon-192.png">
</head>
<body class="bg-slate-100 min-h-screen text-slate-800">
  <div id="loginBox" class="min-h-screen flex items-center justify-center p-4 bg-gradient-to-br from-slate-950 via-slate-800 to-red-900">
    <div class="bg-white w-full max-w-md rounded-3xl shadow-2xl p-8 border">
      <div class="text-center mb-6">
        <div class="w-16 h-16 mx-auto bg-red-600 text-white rounded-2xl flex items-center justify-center text-3xl"><i class="fa-solid fa-shield-halved"></i></div>
        <h1 class="text-2xl font-black mt-4">Tech CND Admin</h1>
        <p class="text-sm text-gray-500">Product, Stock, Discount, Image Management</p>
      </div>
      <input id="pin" type="password" class="border w-full rounded-xl p-3 focus:outline-red-500" placeholder="Admin PIN">
      <button onclick="login()" class="mt-4 w-full bg-red-600 hover:bg-red-700 text-white rounded-xl p-3 font-bold">Login</button>
      <p class="text-xs text-gray-400 mt-3">Default PIN: <b>1234</b> — publish করার আগে code এর PIN বদলে দেবেন।</p>
    </div>
  </div>

  <div id="panel" class="hidden">
    <header class="bg-slate-950 text-white sticky top-0 z-20 shadow">
      <div class="max-w-7xl mx-auto p-4 flex flex-wrap gap-3 justify-between items-center">
        <div><h1 class="font-black text-xl">TECH CND Admin Panel</h1><p class="text-xs text-slate-300">Admin panel আলাদা — public website এ কোনো link নেই</p></div>
        <div class="flex flex-wrap gap-2"><button id="productTabBtn" onclick="showTab('products')" class="bg-red-600 px-5 py-2 rounded-lg font-bold">Products</button><button id="orderTabBtn" onclick="showTab('orders')" class="bg-white/10 px-5 py-2 rounded-lg font-bold">Orders</button><button onclick="seedDemo()" class="bg-white/10 px-4 py-2 rounded-lg font-bold">Load 120 Products</button><button onclick="logout()" class="bg-red-600 px-4 py-2 rounded-lg font-bold">Logout</button></div>
      </div>
    </header>

    <main class="max-w-7xl mx-auto p-4">
      <div class="bg-white rounded-2xl shadow border p-2 mb-4 flex gap-2 sticky top-[82px] z-10">
        <button id="productTabBtn2" onclick="showTab('products')" class="flex-1 bg-red-600 text-white rounded-xl py-3 font-black"><i class="fa-solid fa-boxes-stacked"></i> Product</button>
        <button id="orderTabBtn2" onclick="showTab('orders')" class="flex-1 bg-slate-100 text-slate-900 rounded-xl py-3 font-black"><i class="fa-solid fa-truck-fast"></i> Order</button>
      </div>
      <div id="productsTab">
      <section class="grid md:grid-cols-4 gap-4 mb-4">
        <div class="bg-white rounded-2xl p-4 shadow border"><p class="text-xs text-gray-500">Total Products</p><b id="statTotal" class="text-3xl">0</b></div>
        <div class="bg-white rounded-2xl p-4 shadow border"><p class="text-xs text-gray-500">In Stock</p><b id="statStock" class="text-3xl text-green-600">0</b></div>
        <div class="bg-white rounded-2xl p-4 shadow border"><p class="text-xs text-gray-500">Low Stock</p><b id="statLow" class="text-3xl text-yellow-600">0</b></div>
        <div class="bg-white rounded-2xl p-4 shadow border"><p class="text-xs text-gray-500">Out of Stock</p><b id="statOut" class="text-3xl text-red-600">0</b></div>
      </section>

      <section class="grid lg:grid-cols-3 gap-4">
        <div class="bg-white rounded-2xl p-5 shadow border lg:col-span-1">
          <h2 id="formTitle" class="font-black text-lg mb-4">Add Product</h2>
          <input type="hidden" id="editId">
          <label class="text-xs font-bold">Product Name</label>
          <input id="pname" class="border rounded-xl p-3 w-full mb-3" placeholder="Canon IR 2525 Machine">
          <label class="text-xs font-bold">Category</label>
          <select id="pcat" class="border rounded-xl p-3 w-full mb-3"><option>Canon Machines</option><option>Spare Parts</option><option>Toner & Drum</option><option>Fuser Parts</option><option>Accessories</option></select>
          <div class="grid grid-cols-2 gap-3">
            <div><label class="text-xs font-bold">MRP / Old Price</label><input id="pmrp" type="number" class="border rounded-xl p-3 w-full mb-3" placeholder="65000"></div>
            <div><label class="text-xs font-bold">Selling Price</label><input id="pprice" type="number" class="border rounded-xl p-3 w-full mb-3" placeholder="55000"></div>
          </div>
          <div class="grid grid-cols-2 gap-3">
            <div><label class="text-xs font-bold">Discount %</label><input id="pdiscount" type="number" class="border rounded-xl p-3 w-full mb-3" placeholder="10"></div>
            <div><label class="text-xs font-bold">Stock Qty</label><input id="pqty" type="number" class="border rounded-xl p-3 w-full mb-3" placeholder="5"></div>
          </div>
          <label class="text-xs font-bold">Stock Status</label>
          <select id="pstock" class="border rounded-xl p-3 w-full mb-3"><option>In Stock</option><option>Limited</option><option>Low Stock</option><option>Out of Stock</option></select>
          <label class="text-xs font-bold">Product Details</label>
          <textarea id="pdetails" class="border rounded-xl p-3 w-full mb-3 h-24" placeholder="Condition, warranty, service details..."></textarea>
          <label class="text-xs font-bold">Product Image Upload</label>
          <input id="pimageFile" type="file" accept="image/*" class="border rounded-xl p-3 w-full mb-3" onchange="previewImage(event)">
          <input id="pimage" class="border rounded-xl p-3 w-full mb-3" placeholder="Image URL or uploaded image data" readonly>
          <img id="imgPreview" src="tech-cnd-placeholder.svg" class="w-full h-40 object-contain bg-gray-50 rounded-xl border mb-3 p-2">
          <div class="grid grid-cols-2 gap-3">
            <button onclick="saveProduct()" class="bg-red-600 hover:bg-red-700 text-white rounded-xl p-3 w-full font-bold"><i class="fa-solid fa-floppy-disk"></i> Save</button>
            <button onclick="resetForm()" class="bg-slate-900 text-white rounded-xl p-3 w-full font-bold">Reset</button>
          </div>
          <p class="text-xs text-gray-500 mt-3">এই admin data Google Sheet backend এ save/load হবে। Image upload base64 হিসেবে save হয়।</p>
        </div>

        <div class="bg-white rounded-2xl p-5 shadow border lg:col-span-2">
          <div class="flex flex-wrap gap-3 justify-between items-center mb-4">
            <h2 class="font-black text-lg">Product List <span class="text-xs text-gray-500 font-semibold">(120 default products editable)</span></h2>
            <div class="flex gap-2"><input id="searchAdmin" oninput="render()" class="border rounded-xl p-2" placeholder="Search product"><button onclick="exportProducts()" class="bg-green-600 text-white rounded-xl px-3 font-bold">Export</button><button onclick="clearProducts()" class="text-red-600 font-bold text-sm">Clear</button></div>
          </div>
          <div id="list" class="grid md:grid-cols-2 gap-3"></div>
        </div>
      </section>

      </div>

      <div id="ordersTab" class="hidden">
      <section class="grid lg:grid-cols-3 gap-4 mt-4">
        <div id="orderSection" class="bg-white rounded-2xl p-5 shadow border lg:col-span-1 ring-2 ring-blue-100">
          <h2 class="font-black text-lg mb-1">Order Tracking Update</h2>
          <p class="text-xs text-blue-700 font-bold mb-4">এখান থেকে Order Received / Shipped / Delivered, tracking link এবং delivery boy no add করবেন।</p>
          <label class="text-xs font-bold">Order ID</label>
          <input id="orderId" class="border rounded-xl p-3 w-full mb-3" placeholder="CND123456">
          <label class="text-xs font-bold">Customer Name</label>
          <input id="orderCustomer" class="border rounded-xl p-3 w-full mb-3" placeholder="Customer name">
          <label class="text-xs font-bold">Customer Phone</label>
          <input id="orderPhone" class="border rounded-xl p-3 w-full mb-3" placeholder="Mobile number">
          <label class="text-xs font-bold">Order Status</label>
          <select id="orderStatus" class="border rounded-xl p-3 w-full mb-3">
            <option>Order Received</option><option>Packed</option><option>Shipped</option><option>Out for Delivery</option><option>Delivered</option><option>Cancelled</option>
          </select>
          <label class="text-xs font-bold">Payment Status</label>
          <select id="paymentStatus" class="border rounded-xl p-3 w-full mb-3"><option>Payment Pending</option><option>Paid</option><option>Failed</option><option>Refunded</option></select>
          <label class="text-xs font-bold">Order Amount</label>
          <input id="orderAmount" type="number" class="border rounded-xl p-3 w-full mb-3" placeholder="Total amount">
          <label class="text-xs font-bold">Tracking Link</label>
          <input id="trackingLink" class="border rounded-xl p-3 w-full mb-3" placeholder="Courier / Google Map tracking link">
          <label class="text-xs font-bold">Delivery Boy Name</label>
          <input id="deliveryName" class="border rounded-xl p-3 w-full mb-3" placeholder="Delivery boy name">
          <label class="text-xs font-bold">Delivery Boy Mobile No</label>
          <input id="deliveryPhone" class="border rounded-xl p-3 w-full mb-3" placeholder="Delivery boy number">
          <label class="text-xs font-bold">Order Note</label>
          <textarea id="orderNote" class="border rounded-xl p-3 w-full mb-3 h-20" placeholder="Shipping note, payment, delivery details..."></textarea>
          <div class="grid grid-cols-2 gap-3">
            <button onclick="saveOrder()" class="bg-blue-700 hover:bg-blue-800 text-white rounded-xl p-3 w-full font-bold">Save Order</button>
            <button onclick="resetOrderForm()" class="bg-slate-900 text-white rounded-xl p-3 w-full font-bold">Reset</button>
          </div>
          <p class="text-xs text-gray-500 mt-3">Customer website-এর Track Order box-এ Order ID দিলে payment QR, payment status, tracking link এবং delivery boy number দেখাবে।</p>
        </div>
        <div class="bg-white rounded-2xl p-5 shadow border lg:col-span-2">
          <div class="flex flex-wrap gap-3 justify-between items-center mb-4">
            <h2 class="font-black text-lg">Orders List</h2>
            <input id="searchOrder" oninput="renderOrders()" class="border rounded-xl p-2" placeholder="Search Order ID / phone / status">
          </div>
          <div id="ordersList" class="grid md:grid-cols-2 gap-3"></div>
        </div>
      </section>

      </div>

      <section class="bg-white rounded-2xl p-5 shadow border mt-4">
        <h2 class="font-black text-lg mb-3">Order / Stock Notes</h2>
        <textarea id="notes" class="border rounded-xl p-3 w-full h-28" placeholder="Order/customer/stock notes..."></textarea>
        <button onclick="saveNotes()" class="mt-3 bg-slate-900 text-white rounded-xl p-3 font-bold">Save Notes</button>
      </section>
    </main>
  </div>

<script>

function showTab(tab){
  const products=document.getElementById('productsTab');
  const orders=document.getElementById('ordersTab');
  const pBtns=[document.getElementById('productTabBtn'),document.getElementById('productTabBtn2')];
  const oBtns=[document.getElementById('orderTabBtn'),document.getElementById('orderTabBtn2')];
  if(tab==='orders'){
    products.classList.add('hidden'); orders.classList.remove('hidden'); renderOrders();
    pBtns.forEach(b=>{if(b){b.className=b.id.endsWith('2')?'flex-1 bg-slate-100 text-slate-900 rounded-xl py-3 font-black':'bg-white/10 px-5 py-2 rounded-lg font-bold'}});
    oBtns.forEach(b=>{if(b){b.className=b.id.endsWith('2')?'flex-1 bg-blue-700 text-white rounded-xl py-3 font-black':'bg-blue-700 px-5 py-2 rounded-lg font-bold'}});
  }else{
    orders.classList.add('hidden'); products.classList.remove('hidden'); render();
    pBtns.forEach(b=>{if(b){b.className=b.id.endsWith('2')?'flex-1 bg-red-600 text-white rounded-xl py-3 font-black':'bg-red-600 px-5 py-2 rounded-lg font-bold'}});
    oBtns.forEach(b=>{if(b){b.className=b.id.endsWith('2')?'flex-1 bg-slate-100 text-slate-900 rounded-xl py-3 font-black':'bg-white/10 px-5 py-2 rounded-lg font-bold'}});
  }
}

const ADMIN_PIN='1234';
const STORAGE_KEY='techCndProducts';
const ORDER_KEY='techCndOrders';
const PLACEHOLDER='tech-cnd-placeholder.svg';
const categories=['Canon Machines','Spare Parts','Toner & Drum','Fuser Parts','Accessories'];
const machineRealPhoto={
  'Canon IR ADV C5235 Color Copier':'images/canon-c3322l.jpg',
  'Canon IR ADV 4525 Machine':'images/canon-619i.jpg',
  'Canon IR ADV 4235 Machine':'images/canon-619i.jpg'
};
const img={
  'Canon Machines':PLACEHOLDER,
  'Spare Parts':'https://images.unsplash.com/photo-1581092160607-ee22621dd758?w=500&auto=format&fit=crop',
  'Toner & Drum':'https://images.unsplash.com/photo-1612815154858-60aa4c59eaa6?w=500&auto=format&fit=crop',
  'Fuser Parts':'https://images.unsplash.com/photo-1537462715879-360eeb61a0bc?w=500&auto=format&fit=crop',
  'Accessories':'https://images.unsplash.com/photo-1544244015-0df4b3ffc6b0?w=500&auto=format&fit=crop'
};
function productImage(name,category,index){
  if(category==='Canon Machines') return machineRealPhoto[name]||PLACEHOLDER;
  const n=(name||'').toLowerCase();
  if(n.includes('toner')||n.includes('drum')) return img['Toner & Drum'];
  if(n.includes('fuser')||n.includes('heater')||n.includes('fixing')) return img['Fuser Parts'];
  return img[category]||PLACEHOLDER;
}
function defaultProducts(){
  const baseProducts=[
    ['Canon IR 2525 Photocopier Machine','Canon Machines',55000],['Canon IR 3225 Photocopier Machine','Canon Machines',68000],['Canon IR 2206N Copier Printer','Canon Machines',62000],['Canon IR 2006N Copier Printer','Canon Machines',52000],['Canon IR ADV 4525 Machine','Canon Machines',125000],['Canon IR ADV 4235 Machine','Canon Machines',115000],['Canon IR ADV C5235 Color Copier','Canon Machines',185000],['Canon IR 3300 Heavy Duty Copier','Canon Machines',48000],
    ['Canon NPG-59 Toner','Toner & Drum',1350],['Canon NPG-51 Toner','Toner & Drum',1250],['Canon NPG-28 Toner','Toner & Drum',950],['Canon NPG-67 Toner Set','Toner & Drum',4800],['OPC Drum Canon IR Series','Toner & Drum',1450],['Drum Unit IR 2525/2530','Toner & Drum',3200],['Developer Powder Canon','Toner & Drum',700],['PCR Roller Canon','Toner & Drum',450],
    ['Fuser Assembly IR 2525','Fuser Parts',8500],['Upper Roller Canon','Fuser Parts',1450],['Lower Pressure Roller','Fuser Parts',1750],['Fixing Film Sleeve','Fuser Parts',1200],['Thermistor Canon Fuser','Fuser Parts',650],['Heater Lamp Canon','Fuser Parts',900],['Fuser Gear Set','Fuser Parts',550],['Separation Claw Set','Fuser Parts',350],
    ['Pickup Roller Set','Spare Parts',550],['Separation Pad','Spare Parts',350],['ADF Roller Kit','Spare Parts',1450],['Transfer Roller','Spare Parts',850],['Registration Roller','Spare Parts',1100],['Main Drive Gear','Spare Parts',750],['DC Controller PCB','Spare Parts',4500],['Power Supply Board','Spare Parts',3800],['Scanner Cable','Spare Parts',550],['Paper Tray Sensor','Spare Parts',450],
    ['USB Printer Cable','Accessories',250],['LAN Cable 5 Meter','Accessories',180],['Power Cable Copier','Accessories',300],['Stabilizer for Copier','Accessories',2500],['Machine Stand','Accessories',2200],['Dust Cover','Accessories',450],['Service Tool Kit','Accessories',1200],['Cleaning Cloth Pack','Accessories',120]
  ];
  let products=baseProducts.map((p,i)=>({id:i+1,name:p[0],category:p[1],price:p[2],mrp:p[2],discount:0,qty:i%6+1,details:'',image:productImage(p[0],p[1],i),stock:i%4===0?'Limited':'In Stock'}));
  const models=['IR 2525','IR 3225','IR 2206','IR 2006N','IR 3300','IR ADV 4525','IR ADV 4235'];
  const parts=['Gear','Sensor','Roller','Bushing','Clutch','Spring','Cable','Tray Lock','Developer Seal','Drum Blade'];
  for(let i=products.length+1;i<=120;i++){
    const cat=i%5===0?'Accessories':i%4===0?'Fuser Parts':i%3===0?'Toner & Drum':'Spare Parts';
    const name=cat==='Spare Parts'?`Canon ${models[i%models.length]} ${parts[i%parts.length]}`:cat==='Fuser Parts'?`Canon ${models[i%models.length]} Fuser ${parts[i%parts.length]}`:cat==='Toner & Drum'?`Canon ${models[i%models.length]} Drum/Toner Kit`:`Copier ${parts[i%parts.length]} Accessory`;
    const price=300+(i*73)%9500;
    products.push({id:i,name,category:cat,price,mrp:price,discount:0,qty:i%8+1,details:'',image:productImage(name,cat,i),stock:i%7===0?'Limited':'In Stock'});
  }
  return products;
}
function login(){ if(document.getElementById('pin').value===ADMIN_PIN){localStorage.setItem('techCndAdmin','yes');showPanel()}else alert('Wrong PIN') }
function logout(){localStorage.removeItem('techCndAdmin');location.reload()}
function showPanel(){loginBox.classList.add('hidden');panel.classList.remove('hidden');if(!getProducts().length) seedDemo(true);notes.value=localStorage.getItem('techCndNotes')||'';render();renderOrders()}
function getProducts(){try{return JSON.parse(localStorage.getItem(STORAGE_KEY)||'[]')}catch(e){return []}}
function setProducts(p){localStorage.setItem(STORAGE_KEY,JSON.stringify(p));render()}
function calcPrice(){let price=Number(pprice.value||0), mrp=Number(pmrp.value||0), dis=Number(pdiscount.value||0); if(!price && mrp && dis){pprice.value=Math.round(mrp-(mrp*dis/100));}}
pdiscount.addEventListener('input',calcPrice); pmrp.addEventListener('input',calcPrice);
function previewImage(e){const file=e.target.files[0]; if(!file)return; const reader=new FileReader(); reader.onload=()=>{pimage.value=reader.result; imgPreview.src=reader.result}; reader.readAsDataURL(file)}
function saveProduct(){
  if(!pname.value.trim()){alert('Product name দিন');return}
  let arr=getProducts();
  const id=editId.value || 'P'+Date.now();
  const item={id,name:pname.value.trim(),category:pcat.value,mrp:Number(pmrp.value||pprice.value||0),price:Number(pprice.value||0),discount:Number(pdiscount.value||0),qty:Number(pqty.value||0),stock:pstock.value,details:pdetails.value.trim(),image:pimage.value||PLACEHOLDER};
  const idx=arr.findIndex(x=>String(x.id)===String(id)); idx>=0?arr[idx]=item:arr.unshift(item); setProducts(arr); resetForm(); alert('Product saved. Public website refresh করলে updated product show হবে।');
}
function editProduct(id){const x=getProducts().find(p=>String(p.id)===String(id)); if(!x)return; editId.value=x.id; formTitle.textContent='Edit Product #'+x.id; pname.value=x.name; pcat.value=x.category; pmrp.value=x.mrp||''; pprice.value=x.price||''; pdiscount.value=x.discount||''; pqty.value=x.qty||0; pstock.value=x.stock||'In Stock'; pdetails.value=x.details||''; pimage.value=x.image||''; imgPreview.src=x.image||PLACEHOLDER; scrollTo({top:0,behavior:'smooth'})}
function resetForm(){editId.value=''; formTitle.textContent='Add Product'; ['pname','pmrp','pprice','pdiscount','pqty','pdetails','pimage'].forEach(id=>document.getElementById(id).value=''); pcat.value='Canon Machines'; pstock.value='In Stock'; pimageFile.value=''; imgPreview.src=PLACEHOLDER}
function delProduct(id){if(!confirm('Delete this product?'))return; setProducts(getProducts().filter(p=>String(p.id)!==String(id)))}
function changeStock(id,status){let arr=getProducts(); const x=arr.find(p=>String(p.id)===String(id)); if(x){x.stock=status; if(status==='Out of Stock')x.qty=0; setProducts(arr)}}
function render(){let arr=getProducts(); const q=(searchAdmin?.value||'').toLowerCase(); const shown=arr.filter(x=>((x.name||'')+(x.category||'')+(x.stock||'')+String(x.id)).toLowerCase().includes(q)); statTotal.textContent=arr.length; statStock.textContent=arr.filter(x=>x.stock==='In Stock').length; statLow.textContent=arr.filter(x=>x.stock==='Low Stock'||x.stock==='Limited').length; statOut.textContent=arr.filter(x=>x.stock==='Out of Stock').length; list.innerHTML=shown.length?shown.map(x=>`<div class='border rounded-2xl p-3 flex gap-3 bg-white'><img src='${x.image||PLACEHOLDER}' onerror="this.src='${PLACEHOLDER}'" class='w-24 h-24 object-contain bg-gray-50 rounded-xl border p-1'><div class='flex-1'><div class='flex justify-between gap-2'><b>#${x.id} ${x.name}</b><span class='text-[10px] px-2 py-1 rounded h-fit ${x.stock==='Out of Stock'?'bg-red-100 text-red-700':'bg-green-100 text-green-700'}'>${x.stock}</span></div><p class='text-xs text-gray-500'>${x.category} • Qty: ${x.qty||0}</p><p class='font-black text-red-600'>₹${Number(x.price||0).toLocaleString('en-IN')} ${x.discount?`<span class='text-xs text-green-600'>${x.discount}% OFF</span>`:''}</p><p class='text-xs text-gray-500 line-clamp-1'>${x.details||''}</p><div class='flex flex-wrap gap-2 mt-2'><button onclick="editProduct('${x.id}')" class='bg-slate-900 text-white text-xs px-3 py-1 rounded'>Edit + Image</button><button onclick="delProduct('${x.id}')" class='bg-red-600 text-white text-xs px-3 py-1 rounded'>Delete</button><button onclick="changeStock('${x.id}','In Stock')" class='border text-xs px-2 py-1 rounded'>In</button><button onclick="changeStock('${x.id}','Low Stock')" class='border text-xs px-2 py-1 rounded'>Low</button><button onclick="changeStock('${x.id}','Out of Stock')" class='border text-xs px-2 py-1 rounded'>Out</button></div></div></div>`).join(''):'<p class="text-gray-500">No products added yet. Load 120 Products চাপুন।</p>'}
function clearProducts(){if(confirm('Clear all admin products?'))setProducts([])}

function getOrders(){try{return JSON.parse(localStorage.getItem(ORDER_KEY)||'[]')}catch(e){return []}}
function setOrders(o){localStorage.setItem(ORDER_KEY,JSON.stringify(o));renderOrders()}
function saveOrder(){
  const id=(orderId.value||'').trim().toUpperCase();
  if(!id){alert('Order ID দিন');return}
  const item={id,customer:orderCustomer.value.trim(),phone:orderPhone.value.trim(),status:orderStatus.value,paymentStatus:paymentStatus.value,amount:Number(orderAmount.value||0),tracking:trackingLink.value.trim(),deliveryName:deliveryName.value.trim(),deliveryPhone:deliveryPhone.value.trim(),note:orderNote.value.trim(),updated:new Date().toLocaleString('en-IN')};
  let arr=getOrders(); const i=arr.findIndex(x=>String(x.id).toUpperCase()===id); i>=0?arr[i]=item:arr.unshift(item); setOrders(arr); resetOrderForm(); alert('Order tracking saved. Customer Track Order থেকে দেখতে পারবে।');
}
function editOrder(id){const x=getOrders().find(o=>String(o.id)===String(id)); if(!x)return; orderId.value=x.id; orderCustomer.value=x.customer||''; orderPhone.value=x.phone||''; orderStatus.value=x.status||'Order Received'; paymentStatus.value=x.paymentStatus||'Payment Pending'; orderAmount.value=x.amount||''; trackingLink.value=x.tracking||''; deliveryName.value=x.deliveryName||''; deliveryPhone.value=x.deliveryPhone||''; orderNote.value=x.note||''; orderId.focus()}
function deleteOrder(id){if(confirm('Delete this order?')) setOrders(getOrders().filter(o=>String(o.id)!==String(id)))}
function resetOrderForm(){['orderId','orderCustomer','orderPhone','orderAmount','trackingLink','deliveryName','deliveryPhone','orderNote'].forEach(id=>document.getElementById(id).value=''); orderStatus.value='Order Received'; paymentStatus.value='Payment Pending'}
function statusClass(s){return s==='Delivered'?'bg-green-100 text-green-700':s==='Shipped'||s==='Out for Delivery'?'bg-blue-100 text-blue-700':s==='Cancelled'?'bg-red-100 text-red-700':'bg-yellow-100 text-yellow-700'}
function renderOrders(){
  const q=(document.getElementById('searchOrder')?.value||'').toLowerCase();
  const arr=getOrders().filter(o=>(o.id+o.customer+o.phone+o.status+(o.paymentStatus||'')+o.deliveryPhone).toLowerCase().includes(q));
  const box=document.getElementById('ordersList'); if(!box)return;
  box.innerHTML=arr.length?arr.map(o=>`<div class='border rounded-2xl p-4 bg-white'><div class='flex justify-between gap-2'><b>${o.id}</b><span class='text-[10px] px-2 py-1 rounded h-fit ${statusClass(o.status)}'>${o.status}</span></div><p class='text-xs text-gray-500 mt-1'>Customer: ${o.customer||'-'} • ${o.phone||'-'}</p><p class='text-xs mt-1'><b>Payment:</b> <span class='font-bold ${o.paymentStatus==='Paid'?'text-green-700':'text-red-600'}'>${o.paymentStatus||'Payment Pending'}</span> ${o.amount?`• ₹${Number(o.amount).toLocaleString('en-IN')}`:''}</p><p class='text-xs mt-1'><b>Delivery:</b> ${o.deliveryName||'-'} ${o.deliveryPhone?`• ${o.deliveryPhone}`:''}</p>${o.tracking?`<p class='text-xs mt-1'><b>Tracking:</b> ${o.tracking}</p>`:''}${o.note?`<p class='text-xs text-gray-600 mt-1'>${o.note}</p>`:''}<p class='text-[10px] text-gray-400 mt-1'>Updated: ${o.updated||''}</p><div class='flex gap-2 mt-3'><button onclick="editOrder('${o.id}')" class='bg-slate-900 text-white text-xs px-3 py-1 rounded'>Edit</button><button onclick="deleteOrder('${o.id}')" class='bg-red-600 text-white text-xs px-3 py-1 rounded'>Delete</button></div></div>`).join(''):'<p class="text-gray-500">No orders yet. Customer order করলে auto save হবে, অথবা manual Order ID add করুন।</p>';
}

function saveNotes(){localStorage.setItem('techCndNotes',notes.value);alert('Saved')}
function exportProducts(){const blob=new Blob([JSON.stringify(getProducts(),null,2)],{type:'application/json'}); const a=document.createElement('a'); a.href=URL.createObjectURL(blob); a.download='tech-cnd-products.json'; a.click()}
function seedDemo(silent=false){if(getProducts().length && !silent && !confirm('বর্তমান list replace করে আবার 120 products load করবেন?'))return; setProducts(defaultProducts()); if(!silent) alert('120 products loaded. এখন যেকোনো product Edit + Image থেকে image upload/edit করুন।')}
if(localStorage.getItem('techCndAdmin')==='yes')showPanel();
</script>

<script>
// ===== Google Sheet Backend Connected Version =====
const API_URL = "https://script.google.com/macros/s/AKfycbxSdAqsOtxQdHlcb_zyO91HWdb8fl2HQ3Go7NXCh_0R0wCGoofGGYuz20FtACzfDtVW/exec";
let sheetProducts = [];
let sheetOrders = [];
function normalizeProduct(p,i=0){return {id:p.id||('P'+Date.now()+i),name:p.name||'',category:p.category||'Spare Parts',mrp:Number(p.mrp||p.price||0),price:Number(p.price||0),discount:Number(p.discount||0),qty:Number(p.qty||p.stockQty||0),stock:p.stock||p.stockStatus||'In Stock',details:p.details||'',image:p.image||PLACEHOLDER}}
function normalizeOrder(o){return {id:o.id||o.orderId||'',customer:o.customer||o.customerName||'',phone:o.phone||o.mobile||'',status:o.status||o.orderStatus||'Order Received',paymentStatus:o.paymentStatus||'Payment Pending',amount:Number(o.amount||0),tracking:o.tracking||o.trackingLink||'',deliveryName:o.deliveryName||o.deliveryBoyName||'',deliveryPhone:o.deliveryPhone||o.deliveryBoyMobile||'',note:o.note||'',updated:o.updated||o.date||''}}
async function apiGet(action){const c=new AbortController(); const t=setTimeout(()=>c.abort(),8000); try{const r=await fetch(API_URL+'?action='+action+'&t='+Date.now(),{signal:c.signal,cache:'no-store'}); if(!r.ok) throw new Error('API error'); return await r.json();} finally{clearTimeout(t);} }
async function apiPost(data){const c=new AbortController(); const t=setTimeout(()=>c.abort(),10000); try{const r=await fetch(API_URL,{method:'POST',body:JSON.stringify(data),signal:c.signal,cache:'no-store'}); if(!r.ok) throw new Error('API error'); return await r.json();} finally{clearTimeout(t);} }
async function loadProductsFromSheet(){try{const data=await apiGet('products'); sheetProducts=(Array.isArray(data)?data:[]).filter(x=>x&&x.id).map(normalizeProduct); if(!sheetProducts.length){sheetProducts=defaultProducts(); for(const p of sheetProducts) await apiPost({action:'saveProduct',product:toSheetProduct(p)});} localStorage.setItem(STORAGE_KEY,JSON.stringify(sheetProducts)); render();}catch(e){console.warn(e); sheetProducts=JSON.parse(localStorage.getItem(STORAGE_KEY)||'[]'); render(); alert('Google Sheet load হয়নি। Deploy setting Anyone আছে কিনা দেখুন। Local data দেখানো হচ্ছে।')}}
async function loadOrdersFromSheet(){try{const data=await apiGet('orders'); sheetOrders=(Array.isArray(data)?data:[]).filter(x=>x&&(x.orderId||x.id)).map(normalizeOrder); localStorage.setItem(ORDER_KEY,JSON.stringify(sheetOrders)); renderOrders();}catch(e){console.warn(e); sheetOrders=JSON.parse(localStorage.getItem(ORDER_KEY)||'[]'); renderOrders();}}
function toSheetProduct(p){return {id:p.id,name:p.name,category:p.category,mrp:p.mrp,price:p.price,discount:p.discount,stockQty:p.qty,stockStatus:p.stock,image:p.image,details:p.details}}
function toSheetOrder(o){return {orderId:o.id,customerName:o.customer,mobile:o.phone,address:o.address||'',productName:o.productName||'',amount:o.amount,paymentStatus:o.paymentStatus,orderStatus:o.status,trackingLink:o.tracking,deliveryBoyName:o.deliveryName,deliveryBoyMobile:o.deliveryPhone}}
function getProducts(){return sheetProducts.length?sheetProducts:JSON.parse(localStorage.getItem(STORAGE_KEY)||'[]')}
function getOrders(){return sheetOrders.length?sheetOrders:JSON.parse(localStorage.getItem(ORDER_KEY)||'[]')}
function setProducts(p){sheetProducts=p; localStorage.setItem(STORAGE_KEY,JSON.stringify(p)); render()}
function setOrders(o){sheetOrders=o; localStorage.setItem(ORDER_KEY,JSON.stringify(o)); renderOrders()}
function login(){ if(document.getElementById('pin').value===ADMIN_PIN){localStorage.setItem('techCndAdmin','yes');showPanel()}else alert('Wrong PIN') }
function showPanel(){loginBox.classList.add('hidden');panel.classList.remove('hidden');notes.value=localStorage.getItem('techCndNotes')||''; render(); renderOrders(); loadProductsFromSheet(); loadOrdersFromSheet();}
async function saveProduct(){
  if(!pname.value.trim()){alert('Product name দিন');return}
  const id=editId.value || 'P'+Date.now();
  const item={id,name:pname.value.trim(),category:pcat.value,mrp:Number(pmrp.value||pprice.value||0),price:Number(pprice.value||0),discount:Number(pdiscount.value||0),qty:Number(pqty.value||0),stock:pstock.value,details:pdetails.value.trim(),image:pimage.value||PLACEHOLDER};
  let arr=getProducts(); const idx=arr.findIndex(x=>String(x.id)===String(id)); idx>=0?arr[idx]=item:arr.unshift(item); setProducts(arr);
  try{const res=await apiPost({action:'saveProduct',product:toSheetProduct(item)}); if(!res.ok) throw new Error('save failed'); alert('Product Google Sheet এ saved হয়েছে।'); resetForm(); await loadProductsFromSheet();}catch(e){console.warn(e); alert('Sheet save problem। Local save হয়েছে। Apps Script deploy check করুন।'); resetForm();}
}
async function delProduct(id){if(!confirm('Delete this product?'))return; setProducts(getProducts().filter(p=>String(p.id)!==String(id))); try{await apiPost({action:'deleteProduct',id}); await loadProductsFromSheet();}catch(e){alert('Sheet delete problem।')}}
async function changeStock(id,status){let arr=getProducts(); const x=arr.find(p=>String(p.id)===String(id)); if(x){x.stock=status; if(status==='Out of Stock')x.qty=0; setProducts(arr); try{await apiPost({action:'saveProduct',product:toSheetProduct(x)});}catch(e){console.warn(e)}}}
async function seedDemo(silent=false){if(getProducts().length && !silent && !confirm('বর্তমান Sheet product replace/append করে 120 products load করবেন?'))return; const arr=defaultProducts(); setProducts(arr); try{for(const p of arr) await apiPost({action:'saveProduct',product:toSheetProduct(p)}); if(!silent) alert('120 products Google Sheet এ loaded হয়েছে।')}catch(e){if(!silent) alert('Sheet save problem। Local loaded হয়েছে।')}}
async function saveOrder(){
  const id=(orderId.value||'').trim().toUpperCase(); if(!id){alert('Order ID দিন');return}
  const item={id,customer:orderCustomer.value.trim(),phone:orderPhone.value.trim(),status:orderStatus.value,paymentStatus:paymentStatus.value,amount:Number(orderAmount.value||0),tracking:trackingLink.value.trim(),deliveryName:deliveryName.value.trim(),deliveryPhone:deliveryPhone.value.trim(),note:orderNote.value.trim(),updated:new Date().toLocaleString('en-IN')};
  let arr=getOrders(); const i=arr.findIndex(x=>String(x.id).toUpperCase()===id); i>=0?arr[i]=item:arr.unshift(item); setOrders(arr);
  try{const res=await apiPost({action:'updateOrder',order:toSheetOrder(item)}); if(!res.ok) throw new Error('not found'); alert('Order Google Sheet এ updated হয়েছে।'); resetOrderForm(); await loadOrdersFromSheet();}catch(e){console.warn(e); alert('Order update হয়নি। Order ID Sheet-এ আছে কিনা দেখুন। Local save হয়েছে।'); resetOrderForm();}
}
async function deleteOrder(id){if(confirm('Delete not supported in Sheet code. Cancelled status করবেন?')){const o=getOrders().find(x=>String(x.id)===String(id)); if(o){o.status='Cancelled'; await apiPost({action:'updateOrder',order:toSheetOrder(o)}); await loadOrdersFromSheet();}}}
function exportProducts(){const blob=new Blob([JSON.stringify(getProducts(),null,2)],{type:'application/json'}); const a=document.createElement('a'); a.href=URL.createObjectURL(blob); a.download='tech-cnd-products.json'; a.click()}
if(localStorage.getItem('techCndAdmin')==='yes'){setTimeout(showPanel,200)};
</script>

<script>
  if ("serviceWorker" in navigator) {
    window.addEventListener("load", function () {
      navigator.serviceWorker.register("sw.js").catch(function (error) {
        console.log("Service Worker registration failed:", error);
      });
    });
  }

  let deferredInstallPrompt;
  window.addEventListener("beforeinstallprompt", function (event) {
    event.preventDefault();
    deferredInstallPrompt = event;
    const btn = document.getElementById("installAppBtn");
    if (btn) btn.classList.remove("hidden");
  });

  async function installTechCndApp() {
    if (!deferredInstallPrompt) {
      alert("Install option browser menu থেকে Add to Home Screen / Install App চাপুন।");
      return;
    }
    deferredInstallPrompt.prompt();
    await deferredInstallPrompt.userChoice;
    deferredInstallPrompt = null;
    const btn = document.getElementById("installAppBtn");
    if (btn) btn.classList.add("hidden");
  }
</script>
</body>
</html>