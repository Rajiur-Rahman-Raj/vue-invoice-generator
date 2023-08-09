<script setup>
import { reactive } from "vue"

    const data = reactive({     
      invoiceNumber:'',
      sender:'',
      billTo:'',
      shipTo:'',
      date:'',
      dueDate:'',
      additionalNote:'',
      items:[
        {
          description:'',
          quantity:'',
          rate:'',
          amount:''
        }
      ],
      notes:'',
      terms:'',
      subtotal:'',
      tax:'',
      discount:'',
      total:'',
    })

    function addItem(){
      data.items.push({
        description:'',
          quantity:'',
          rate:'',
          amount:''
      })
    }

    function subtotal(){
      let subTotal = 0;
      data.items.forEach(item => {
        subTotal += item.amount
      })
      data.subtotal = subTotal
      return subTotal;
    }

    function totalAmount(){
      let tax = data.subtotal * data.tax / 100;
      let totalAmountWithTax = tax + data.subtotal

      let discount = data.discount * totalAmountWithTax / 100;
      let totalFinalAmount = totalAmountWithTax - discount;
      return totalFinalAmount;
    }

    function removeItem(index){
        data.items.splice(index);
    }


</script>

<template>
  <section class="mx-auto container bg-white border border-gray-400 min-h-screen p-12">
        <div class="flex justify-between">
            <div class="flex flex-col space-y-5 w-1/2s">
                <div class=" ">
                    <img class="w-40" src="https://www.shutterstock.com/image-vector/invoice-typographic-stamp-sign-badge-600w-1027820257.jpg" alt="">
                    {{ data }}
                </div>
                <p class="mt-5">
                    Sender
                </p>
                <textarea name="" id="" cols="30" rows="2" v-model="data.sender"></textarea>
                <div class="flex space-x-2">
                    <div class="flex flex-col">
                        <span>Bill to</span>
                        <textarea name="" id="" cols="30" rows="2" v-model="data.billTo"></textarea>
                    </div>
                    <div class="flex flex-col">
                        <span>Ship to</span>
                        <textarea name="" id="" cols="30" rows="2" v-model="data.shipTo"></textarea>
                    </div>
                </div>
            </div>
            <div class="flex flex-col w-1/2 items-end">
                <h1 class="mt-12 text-4xl uppercase text-right mb-5">Invoice</h1>
                <input class="w-[200px] text-right" type="text" placeholder="Invoice Number" v-model="data.invoiceNumber">
                <div class="mt-10 flex-y-5 text-right space-y-3 w-full">
                    <p>
                        <span>Date</span>
                        <input class="ml-2 w-[200px] " v-model="data.date">
                    </p>
                    <p>
                        <span>Due Date</span>
                        <input class="ml-2 w-[200px]" v-model="data.dueDate">
                    </p>
                    <p>
                        <span>Additional Note</span>
                        <input class="ml-2 w-[200px]" type="text" v-model="data.additionalNote">
                    </p>
                </div>
            </div>
        </div>
        <div class="mt-20">
            <table class="table-auto w-full">
                <tr class="bg-gray-800 text-left text-white">
                    <th class="p-2 pl-5 w-1/2">Item</th>
                    <th class="p-2">Quantity</th>
                    <th class="p-2">Rate</th>
                    <th class="p-2 w-[200px] text-right pr-5">Amount</th>
                    <th class="p-2">Action</th>
                </tr>
                <tr v-for="(item, index) in data.items" :key="index">
                    <td class="py-1">
                        <input class="w-full pl-5" type="text" placeholder="Description" v-model="item.description" />
                    </td>
                    <td class="">
                        <input class="w-full" type="number" placeholder="Quantity" v-model="item.quantity" />
                    </td>
                    <td class="">
                        <input class="w-full" type="number" placeholder="Rate" v-model="item.rate">
                    </td>
                    <td class="py-1 pr-5 text-right text-gray-800">
                        ${{ item.amount = item.quantity * item.rate }}
                    </td>

                    <td class="py-1 pr-5 text-right text-gray-800">
                        <button class="bg-red-800 text-white" @click="removeItem(index)">Delete</button>
                    </td>
                </tr>
            </table>
            <button class="mt-5 bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded" @click="addItem()">
                Add More
            </button>
        </div>
        <div class="mt-[200px]">
            <div class="flex justify-between">
                <div class="flex flex-col space-y-5 w-1/2">
                    <span>Notes</span>
                    <textarea name="" id="" cols="30" rows="2" v-model="data.notes"></textarea>
                    <span>Terms</span>
                    <textarea name="" id="" cols="30" rows="2" v-model="data.terms"></textarea>
                </div>
                <div class="flex flex-col w-1/2 items-end">
                    <div class="mt-10 flex-y-5 text-right space-y-3 w-full">
                        <p>
                            <span>Subtotal</span>
                            <input readonly class="focus:ring-0 focus:ring-offset-0 text-right ml-2 pr-4 w-[200px] border-0" placeholder="Subtotal" :value="subtotal()">
                        </p>
                        <p>
                            <span>Tax</span>
                            <input type="number" class="tax text-right w-[200px] ml-2" v-model="data.tax">
                        </p>

                        <p>
                            <span>Discount</span>
                            <input type="number" class="tax text-right w-[200px] ml-2" v-model="data.discount">
                        </p>

                        <p>
                            <span>Total</span>
                            <input readonly class="focus:ring-0 focus:ring-offset-0 text-right ml-2 pr-4 w-[200px] border-0" placeholder="Total" :value="totalAmount()">
                        </p>
                      
                    </div>
                </div>
            </div>
        </div>

    </section>

</template>

<style scoped>
input,
        textarea {
            border: 1px solid #ddd !important;
            padding: 5px;
            border-radius: 5px;
        }

        input.tax::-webkit-outer-spin-button,
        input.tax::-webkit-inner-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }
</style>
