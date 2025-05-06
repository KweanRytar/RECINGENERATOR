<script setup>
import { ref , nextTick} from 'vue'
import html2canvas from 'html2canvas'


const companyName = ref("")
const companyAddress = ref("")
const CompanyNumber = ref("")
const CompanyEmail = ref("")
const CompanyWebsite = ref("")
const invoiceNumber = ref("")
const paymentMethod = ref("")
const IssueDate = ref("")
const DeliveryDate = ref("")
const DueDate = ref("")
const ClientAddress = ref("")
const ClientName = ref("")
const secondaryColor = ref("")
const primaryColor = ref("")
const totalAmount = ref(0)
const imageUrl = ref("")
const imageUrlRec = ref("")

const invoiceOrder = ref(false)
const receiptOrder = ref(false)
const generatingInvoice = ref(false)
const generatingReceipt = ref(false)

const handleInvoiceGeneration = () => {
  if (
    companyAddress.value &&
    companyName.value &&
    CompanyNumber.value &&
    CompanyEmail.value &&
    CompanyWebsite.value &&
    invoiceNumber.value &&
    paymentMethod.value &&
    DeliveryDate.value &&
    DueDate.value &&
    ClientAddress.value &&
    ClientName.value &&
    secondaryColor.value &&
    primaryColor.value &&
    imageUrl.value
  ) {
    invoiceOrder.value = false
    generatingInvoice.value = true
  } else {
    alert("Please complete all the fields")
  }
}

const handleImageUpload = (e) => {
  const file = e.target.files[0]
  if (file) {
    const reader = new FileReader()
    reader.onload = (e) => {
      imageUrl.value = e.target.result
    }
    reader.readAsDataURL(file)
  }
}

const itemName = ref("")
const itemDescription = ref("")
const itemQuantity = ref(0)
const itemUnitPrice = ref(0)
const itemTotal = ref(0)
const items = ref([])

const resetItemFields = () => {
  itemName.value = ""
  itemDescription.value = ""
  itemQuantity.value = ""
  itemUnitPrice.value = ""
}

const add = () => {
  if (
    itemName.value &&
    itemDescription.value &&
    itemQuantity.value &&
    itemUnitPrice.value
  ) {
    itemTotal.value = itemQuantity.value * itemUnitPrice.value
    items.value.push({
      name: itemName.value,
      description: itemDescription.value,
      quantity: Number(itemQuantity.value),
      unitPrice: Number(itemUnitPrice.value),
      total: itemTotal.value
    })
    totalAmount.value += itemTotal.value
    resetItemFields()
  } else {
    alert("Please fill all fields")
  }
}

const invoiceRef = ref(null)

import domtoimage from 'dom-to-image';
import jsPDF from 'jspdf';

const receipt = ref(null)
const downloadReceipt = async () => {
  try {
    const receiptRef = receipt.value;
    if (!receiptRef) return;

    const dataUrl = await domtoimage.toPng(receiptRef);
    const pdf = new jsPDF('p', 'pt', 'a4');
    const imgProps = pdf.getImageProperties(dataUrl);
    const pdfWidth = pdf.internal.pageSize.getWidth();
    const pdfHeight = (imgProps.height * pdfWidth) / imgProps.width;

    pdf.addImage(dataUrl, 'PNG', 0, 0, pdfWidth, pdfHeight);
    pdf.save(clientNameRec.value + '-' + recNumber.value);
  } catch (error) {
    console.error('Error generating PDF:', error);
  }
};


const downloadInvoice = async () => {
  const invoice = invoiceRef.value;
  if (!invoice) return;

  domtoimage.toPng(invoice)
    .then((dataUrl) => {
      const pdf = new jsPDF('p', 'pt', 'a4');
      const imgProps = pdf.getImageProperties(dataUrl);
      const pdfWidth = pdf.internal.pageSize.getWidth();
      const pdfHeight = (imgProps.height * pdfWidth) / imgProps.width;
      pdf.addImage(dataUrl, 'PNG', 0, 0, pdfWidth, pdfHeight);
      pdf.save(ClientName + '-' + invoiceNumber);
    })
    .catch((error) => {
      console.error('Error generating PDF:', error);
    });
};



const backToEdit = () => {
  invoiceOrder.value = true
  generatingInvoice.value = false
}
// receipt logic

const logoUrl = ref()
const handleLogoUpload = (e) =>{
  const image = e.target.files[0]
  if(image){
    const reader = new FileReader()
      reader.onload = (e) => {
logoUrl.value = e.target.result
      }

    reader.readAsDataURL(image)
  }
}

const recSignature = ref()
const handleSignatureUploadRec = (e) => {
const signature = e.target.files[0]
if(signature){
  const reader = new FileReader()
  reader.onload = (e)=>{
    recSignature.value = e.target.result
  }
  reader.readAsDataURL(signature)
}
}
const companyNameRec = ref("Noble")
const companyAddressRec = ref("Adaora street")
const CompanyEmailRec = ref("Noble@gmail.com")
const CompanyPhoneNumberRec= ref(906145355)
const clientNameRec = ref("Enugu State")
const clientAddressRec = ref("Ukwuoma Street")
const transactionDate = ref()
const recNumber = ref(2)

// adding items in receipt
const itemNameRec = ref("")
const itemDescriptionRec = ref("")
const itemQuantityRec = ref()
const itemUnitPriceRec = ref()
const itemTotalRec = ref()
const totalAmountRec = ref(0)
const recItems = ref([])
const resetItemFieldRec = () => {
  itemNameRec.value = ""
  itemDescriptionRec.value= ""
  itemQuantityRec.value = ""
  itemUnitPriceRec.value = ""
  
}
const addRec = () => {
  if(
    itemNameRec.value &&
     itemDescriptionRec.value && 
     itemQuantityRec.value && 
     itemUnitPriceRec.value){

  itemTotalRec.value = itemQuantityRec.value * itemUnitPriceRec.value
  recItems.value.push({
    name: itemNameRec.value,
    description: itemDescriptionRec.value,
    quantity: Number(itemQuantityRec.value),
    unitPrice: Number(itemUnitPriceRec.value),
    total: itemTotalRec.value
}) 
totalAmountRec.value += itemTotalRec.value
resetItemFieldRec()
  }
else {
  alert("please fill all item fields")
} 
}

const primaryColorRec = ref()
const secondaryColorRec = ref()
const handleReceiptGeneration = () =>{
  if(
    companyNameRec.value &&
    companyAddressRec.value &&
    CompanyEmailRec.value &&
    transactionDate.value &&
    primaryColorRec.value && 
    secondaryColorRec.value &&
    clientAddressRec.value &&
    clientNameRec.value &&
    recNumber.value &&
    recSignature.value
  ){
    receiptOrder.value = false
generatingReceipt.value = true
  }else{
alert("Please complete all the fields")
  }
}
</script>

<template>
 <div class="flex justify-center bg-blue-500 rounded-2xl gap-6 p-8 m-12" v-show="invoiceOrder === false && generatingInvoice === false && receiptOrder === false && generatingReceipt === false ">
  <button class="bg-green-500 text-white rounded-2xl p-4"  @click="()=>{
    invoiceOrder = true
    receiptOrder = false
  }">Generate Invoice</button>
  <button class="bg-red-500 text-white rounded-2xl p-4" @click="()=> {
    receiptOrder = true
    invoiceOrder = false
  }">Generate Receipt</button>
 </div>

 <div class="flex flex-col m-8" v-show="invoiceOrder">
 <!-- Getting needed Information -->
  <h1 class="text-2xl text-blue-600 text-center mb-4 font-bold">Generate Invoice</h1>
  <label for="companyName" class="text-sm text-blue-600 font-bold">Enter your Company's Name</label>
   <input type="text" placeholder="Company Name" v-model="companyName" name="companyName" class="w-full border-2 border-gray-300 h-8 p-2 placeholder:text-blue-400 mb-6">

   <label for="companyAddress" class="text-sm text-blue-600 font-bold">Enter your Company's Address</label>
   <input type="text" placeholder="Company Adress" v-model="companyAddress" class="w-full border-2 border-gray-300 h-8 p-2 placeholder:text-blue-400 mb-6">
   
   <label for="CompanyEmail" class="text-sm text-blue-600 font-bold">Enter Your Company's Phone Numer</label>
   <input type="tel" placeholder="Phone Number" v-model="CompanyNumber" class="w-full border-2 border-gray-300 h-8 p-2 placeholder:text-blue-400 mb-6">

   <label for="email" class="text-sm text-blue-600 font-bold" >Enter Your Company's Email Address</label>
   <input type="email" placeholder="Email" v-model="CompanyEmail" class="w-full border-2 border-gray-300 h-8 p-2 placeholder:text-blue-400 mb-6">

   <label for="CompanyWebsite" class="text-sm text-blue-600 font-bold">Enter Your Company's Website</label>
   <input type="text" placeholder="website" v-model="CompanyWebsite" class="w-full border-2 border-gray-300 h-8 p-2 placeholder:text-blue-400 mb-6">

   <label for="invoiceNumber" class="text-sm text-blue-600 font-bold">Enter Invoice Number</label>
   <input type="number" min="1" step="1" placeholder=" invoice number" v-model="invoiceNumber" class="w-full border-2 border-gray-300 h-8 p-2 placeholder:text-blue-400 mb-6">

<label for="" class="text-sm text-blue-600 font-bold">Select a Payment Method</label>
   <select name="paymentMethod" v-model="paymentMethod" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400" >
    <option value="choose">--Select an option---</option>
    <option value="Bank Transer">Bank Transer</option>
    <option value="Cash">Cash</option>
   </select>

   <label for="" class="text-sm text-blue-600 font-bold">Enter Issue Date</label>
   <input type="date" placeholder="Issue Date" v-model="IssueDate" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400">

   <label for="" class="text-sm text-blue-600 font-bold">Enter Delivery Date</label>
   <input type="date" placeholder="Delivery Date" v-model="DeliveryDate" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400">

   <label for="" class="text-sm text-blue-600 font-bold">Enter Due Date</label>
   <input type="date" placeholder="Due Date" v-model="DueDate" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400">


   <label for="" class="text-sm text-blue-600 font-bold">Enter Client's Name</label>
   <input type="text" placeholder="Client Name" v-model="ClientName" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400">

   
   <label for="" class="text-sm text-blue-600 font-bold">Enter Client's Address</label>
   <input type="text" placeholder="Client Address" v-model="ClientAddress" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400">

   <label for="">Upload Your Company's Signatory</label>
    <input type="file" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400"  accept="image/*" @change="handleImageUpload">

   <label for="" class="text-sm text-blue-600 font-bold">Enter Your Brand Primary Colour</label>
   <input type="text" placeholder="Primary Colour" v-model="primaryColor" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400">

   <label for="" class="text-sm text-blue-600 font-bold">Enter Your Brand Secondary Colour</label>
<input type="text" placeholder="Secondary Colour" v-model="secondaryColor" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400">

<div class="bg-amber-600 rounded-2xl m-2 p-4 flex flex-col  justify-center">
  <label for="" class="text-sm text-white">Enter Item Name</label>
  <input type="text" placeholder="enter the item name" v-model="itemName" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400">
  <label for="" class="text-sm text-white">Enter Item description</label>
  <input type="text" placeholder="enter the description" v-model="itemDescription" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400">

  <label for="" class="text-sm text-white">Enter Item Quantity</label>
  <input type="number" min="1" placeholder="enter the quantity"class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400" v-model="itemQuantity">
  <label for="" class="text-sm text-white">Enter unit price</label>
  <input type="number" placeholder="enter the unit price" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400" v-model="itemUnitPrice">
  <button class="bg-transparent border-2 border-gray-300 text-white w-22 rounded-2xl mx-auto hover:bg-blue-600" @click="add">Add Item</button>
</div>
   <button class="bg-blue-600 text-white p-2 cursor-pointer" @click="handleInvoiceGeneration">Generate Invoice</button>
 </div>

 <!-- invoice section -->
  <div ref="invoiceRef" v-show="generatingInvoice === true">
    <div class="mt-10 mr-4 ml-4 mb-0">
  <div class="flex  justify-around w-full" >
  <div class="flex flex-col gap-2">
    <h1 class="text-3xl tracking-wider font-mono font-bold" :style="{color: primaryColor}">{{ companyName}}</h1>
  <h2 class="font-bold text-2xl" :style="{color: secondaryColor}">
        Amount Due: NGN {{totalAmount}}
      </h2>
  </div>
  
  <div class=" flex gap-0">
  
<div class="p-4" :style="{background: secondaryColor}">
  <p class="text-white font-mono text-sm">INVOICE NO: {{ invoiceNumber }}</p>
</div>
<div class="flex flex-col  p-4 " :style="{background: primaryColor}">
<p class="text-sm font-bold text-white">Issue Date</p>
<p class="text-white font-mono">{{ IssueDate }}</p>
<hr class=" text-white mb-4">
<p class="text-sm font-bold text-white text-nowrap">Delivery Date</p>
<p class="text-white font-mono">{{ DeliveryDate }}</p>
<hr class=" text-white mb-4">
<p class="text-sm font-bold text-white text-nowrap">Due Date</p>
<p class="text-white font-mono">{{ DueDate }}</p>

</div>
    </div>
  
      
    
 </div>
 
  <div >
    
    

 </div>
    <hr class="mt-4 mb-2 border-2" :style="{color: secondaryColor}"/>
   <!-- supplier and client information  -->
    <div class="flex justify-between ">
      <!-- supplier information -->
       <div>
<h2 class="m-2 font-bold text-center text-2xl" :style="{color: secondaryColor}">Supplier</h2>
<p class="font-bold m-2 uppercase">{{ companyName }}</p>
<p>{{ companyAddress }}</p>
       </div>
       <div class="w-px h-22 bg-gray-400 m-4"></div>
       <!-- client information -->
        <div>
<h2 class="m-2 font-bold text-center text-2xl" :style="{color: primaryColor}">Client</h2>
<p class="font-bold m-2 uppercase">{{ ClientName }}</p>
<p>{{ ClientAddress }}</p>
        </div>
    </div>
    <!-- thank you section -->
     
    <div class="flex justify-around mt-6 ml-4 gap-x-3">
<div class="flex flex-col">
      <p class="text-gray-500 font-bold">Payment Method</p>
      <p class="font-black">{{ paymentMethod }}</p>
     </div>
     <div>
      <p class="text-3xl font-medium"> THANK YOU FOR YOUR PURCHASE</p>
     </div>
    </div>
    <!-- table section -->
    <div class="mt-6">
      <table class="w-full text-sm text-left text-gray-500">
        <thead class="text-xs text-white uppercase" :style="{background: primaryColor}">
          <tr>
            <th scope="col" class="px-6 py-3 border-2 border-white">Item Name</th>
            <th scope="col" class="px-6 py-3 border-2 border-white">Description</th>
            <th scope="col" class="px-6 py-3 border-2 border-white">Quantity</th>
            <th scope="col" class="px-6 py-3 border-2 border-white">Unit Price</th>
            <th scope="col" class="px-6 py-3 border-2 border-white">Total</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index) in items" :key="index" class="bg-white border-b hover:bg-gray-50">
            <td class="px-6 py-4 font-medium text-gray-900">{{ item.name }}</td>
            <td class="px-6 py-4 font-medium text-gray-900">{{ item.description }}</td>
            <td class="px-6 py-4 font-medium text-gray-900">{{ item.quantity }}</td>
            <td class="px-6 py-4 font-medium text-gray-900">{{ item.unitPrice }}</td>
            <td class="px-6 py-4 font-medium text-gray-900">{{ item.total }}</td>
          </tr>
          <tr class="bg-white border-b hover:bg-gray-50">
            <td colspan="4" class="px-6 py-4 font-medium text-gray-900">Total Amount</td>
            <td class="px-6 py-4 font-medium text-gray-900">{{ totalAmount }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <img  :src="imageUrl" alt="image" class="w-18 mx-auto">
    <p class="text-center font-bold">Director, {{ companyName }}</p>
    <!-- footer section -->
    <div class="flex flex-col justify-between mt-6 rounded-2xl p-4" :style="{background: primaryColor}">
      <p class="text-sm text-gray-500 mb-2 text-center">Thank you for your Trust!</p>
      <p class="text-sm text-gray-500 mb-2 text-center">If you have any questions, feel free to contact us.</p>
      <p class="text-sm text-gray-500 text-center uppercase font-bold m-2">{{ companyName }}</p>
      <hr :style="{color: secondaryColor}" class="border-2 mb-2">
      <div class=" flex justify-around gap-4">
        <p class="text-sm text-gray-500">Phone: {{ CompanyNumber }}</p>
      <p class="text-sm text-gray-500">Email: {{ CompanyEmail }}</p>
      <p class="text-sm text-gray-500">Website: {{ CompanyWebsite }}</p>
      </div>
      
    </div>
 </div>
  </div>
 
 <div class="flex justify-center bg-blue-500 rounded-2xl gap-6 p-8 m-12" v-show="generatingInvoice === true">
  <button class="bg-green-500 text-white rounded-2xl p-4"  @click="()=>{invoiceOrder = true;
    generatingInvoice = false
  }">Generate Invoice</button>
  <button class="bg-yellow-500 text-white rounded-2xl p-4" @click="downloadInvoice">Download invoice as pdf</button>
  <button class="bg-red-500 text-white rounded-2xl p-4" @click="receiptOrder = true">Generate Receipt</button>
 </div>


 <!-- receipt Data gathering section -->
  <div class="flex flex-col ml-6" v-show="receiptOrder=== true ">
    <h1 class="text-2xl text-blue-600 text-center mb-4 font-bold">Generate Receipt</h1>
    <label for="companyL" class="text-sm text-blue-600 font-bold">Upload Company Logo</label>
    <input type="file" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400 "  accept="image/*" @change="handleLogoUpload">
    <label for=""class="text-sm text-blue-600 font-bold">Enter your company's Name </label>
    <input type="text" v-model="companyNameRec" placeholder="Name" class="w-full border-2 border-gray-300 h-8 p-2 placeholder:text-blue-400 mb-6">
    <Label class="text-sm text-blue-600 font-bold">Enter your company's Address</Label>
    <input type="text" v-model="companyAddressRec" placeholder="Address" class="w-full border-2 border-gray-300 h-8 p-2 placeholder:text-blue-400 mb-6">
    <label for="" class="text-sm text-blue-600 font-bold">Enter Compny's Email</label>
    <input type="text" v-model="CompanyEmailRec" placeholder="Email" class="w-full border-2 border-gray-300 h-8 p-2 placeholder:text-blue-400 mb-6">
    <label for="companyL" class="text-sm text-blue-600 font-bold">Upload Company's Signature</label>
    <input type="file" @change="handleSignatureUploadRec" accept="image/*" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400">
    
    <label for="" class="text-sm text-blue-600 font-bold">Enter your cleint's Name </label>
    <input type="email" v-model="clientNameRec" placeholder="Client Name" class="w-full border-2 border-gray-300 h-8 p-2 placeholder:text-blue-400 mb-6">
    <Label class="text-sm text-blue-600 font-bold">Enter your cleint's Address</Label>
    <input type="text" v-model="clientAddressRec" placeholder=" Client Address" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400">
    <label for="" class="text-sm text-blue-600 font-bold">Enter client's Email</label>
    <input type="text" v-model="ClientEmailRec" placeholder="Client Email" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400">
    <label for="" class="text-sm text-blue-600 font-bold">Enter Date of Transaction</label>
    <input type="date" v-model="transactionDate" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400">
    <label for="" class="text-sm text-blue-600 font-bold">Enter Receipt Number</label>
    <input type="number" min="1" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400" v-model="recNumber">
    <label for="" class="text-sm text-blue-600 font-bold">Enter Phone Number</label>
    <input type="tel" v-model="CompanyPhoneNumberRec" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400">
   <label for="" class="text-sm text-blue-600 font-bold">Enter Brand Primary Color</label>
   <input type="text" v-model="primaryColorRec" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400">
   <label for="" class="text-sm text-blue-600 font-bold">Enter Brand Secondary Color</label>
   <input type="text" placeholder="secondaryColor" v-model="secondaryColorRec" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400">
    
    <!-- item/service -->
    <div class="bg-blue-600 rounded-2xl m-2 p-4 flex flex-col  justify-center">
  <label for="" class="text-sm text-white">Enter Item Name</label>
  <input type="text" placeholder="enter the item name" v-model="itemNameRec" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400">
  <label for="" class="text-sm text-white">Enter Item description</label>
  <input type="text" placeholder="enter the description" v-model="itemDescriptionRec" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400">

  <label for="" class="text-sm text-white">Enter Item Quantity</label>
  <input type="number" min="1" placeholder="enter the quantity"class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400" v-model="itemQuantityRec">
  <label for="" class="text-sm text-white">Enter unit price</label>
  <input type="number" placeholder="enter the unit price" class="w-full border-2 border-gray-300 h-12 p-2  mb-6 text-blue-400" v-model="itemUnitPriceRec">
  <button class="bg-transparent border-2 border-gray-300 text-white w-22 rounded-2xl mx-auto hover:bg-blue-600" @click="addRec">Add Item</button>
</div>
    <button class="bg-amber-600 text-white p-2 cursor-pointer" @click="handleReceiptGeneration">Generate Receipt</button>
  </div>

  <!-- receipt template -->
   <div class="mx-auto my-auto p-4" v-show="generatingReceipt === true" ref="receipt">
    <div>
      <div>
      <img :src="logoUrl" alt="" class="rounded-[50%]  p-2 h-10 w-[20]" :style="{
        backgroundColor: primaryColorRec}">
      <h1 class="text-2xl font-bold mt-4" :style="{
        color: primaryColorRec
      }">{{ companyNameRec }}</h1>
      <p class="font-mono capitalize text-amber-700">{{ companyAddressRec }}</p>
      <p class="font-mono text-amber-600 "><span>{{ CompanyPhoneNumberRec }}</span> | {{ CompanyEmailRec }}</p>
    </div>
    
<div class="flex justify-between">
 <div>
<p class="font-bold mt-4 mb-2">RECIEPENT:</p>
<p class="capitalize font-bold" :style="{color: primaryColorRec}">{{ clientNameRec }}</p>
<p>{{ clientAddressRec }}</p>
 </div>
 <div>
<p class="text-white font-semibold rounded-2xl h-12 p-4" :style="{
  backgroundColor: secondaryColorRec
}">Receipt for #{{ recNumber }}</p>
<p class="text-white font-semibold  h-12 w-fit p-2 bg-gray-400">Transaction Date: {{ transactionDate }}</p>
 </div>
  
</div>
    </div>
      
  <div>
    <table class="w-full text-sm text-left text-gray-500 ml-4 mt-6">
      <thead class="text-xs text-white uppercase" :style="{background: secondaryColorRec}">
        <tr>
          <th scope="col" class="px-6 py-3 border-2 border-white">Product/service Name</th>
          <th scope="col" class="px-6 py-3 border-2 border-white">Description</th>
          <th scope="col" class="px-6 py-3 border-2 border-white">Quantity</th>
          <th scope="col" class="px-6 py-3 border-2 border-white">Unit Price</th>
          <th scope="col" class="px-6 py-3 border-2 border-white">Total</th>
        </tr>
       </thead>
       <tbody v-for="(item, index) in recItems" :key="index">
        <tr>
<td class="font-bold capitalize text-center">{{ item.name }}</td>
<td class="font-bold capitalize text-center">{{ item.description }}</td>
<td class="font-bold capitalize text-center">{{ item.quantity }}</td>
<td class="font-bold capitalize text-center">{{ item.unitPrice }}</td>
<td class="font-bold capitalize text-center">{{ item.total }}</td>
        </tr>
        
       </tbody><tr>
          <td colspan="4" class="text-2xl font-bold text-center "> TOTAL</td>
          <td class="text-2xl text-center font-bold">{{ totalAmountRec }}</td>
        </tr>
    </table>
  </div>
  <p class="m-4 text-2xl text-center font-bold">Thanks for your patronage</p>
<div class="flex justify-between mt-8">
  <div>
    <p class="font-bold ml-4">Company's Acknoledgment</p>
  <img :src="recSignature" alt="" class="w-[20] h-16">
  </div>

  


 <div>
  <p class="font-bold ">Customer's Signature</p>

 </div>

    </div>
    
   </div>
  
<div class="flex justify-center bg-blue-500 rounded-2xl gap-6 p-8 m-12" v-if="generatingReceipt === true">
  <button class="bg-green-500 text-white rounded-2xl p-4"  @click="()=>{invoiceOrder = true;
    generatingInvoice = false
    receiptOrder = false
    generatingReceipt = false
  }">Generate Invoice</button>
  <button class="bg-yellow-500 text-white rounded-2xl p-4" @click="downloadReceipt">Download Reciept as pdf</button>
  <button class="bg-red-500 text-white rounded-2xl p-4" @click="()=>{
    receiptOrder = true
    invoiceOrder = false;
    generatingInvoice = false
    generatingReceipt = false

  }">Generate Receipt</button>
 </div>
</template>

<style scoped>

</style>
