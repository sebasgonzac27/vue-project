<script>
import * as data from "./mails";
export default {
  name: "App",
  components: {},
  data: function () {
    return {
      filteredMails: [],
      headers: ["Subject", "From", "To"],
      searchMail: "",
      mailSelected: {},
    };
  },
  watch: {
    searchMail: function () {
      this.filteredMails = data.originalMails.filter((mail) => {
        return (
          mail.from
            .toLowerCase()
            .includes(this.searchMail.toLocaleLowerCase()) ||
          mail.to.toLowerCase().includes(this.searchMail.toLocaleLowerCase()) ||
          mail.subject
            .toLowerCase()
            .includes(this.searchMail.toLocaleLowerCase()) ||
          mail.body.toLowerCase().includes(this.searchMail.toLocaleLowerCase())
        );
      });
    },
  },
  created: function () {
    this.filteredMails = data.originalMails;
  },
  methods: {
    getMail: function (mail) {
      this.mailSelected = mail;
    },
  },
};
</script>

<template>
  <header>
    <div class="flex justify-center bg-slate-900">
      <div class="text-center text-white px-6 md:px-12">
        <h1 class="text-5xl font-bold mt-10 mb-6">Mail Vue App</h1>
        <h3 class="text-3xl font-bold mb-8">Search my mail</h3>
      </div>
    </div>
  </header>
  <div class="mx-4 mb-3 mt-5 xl:w-auto">
    <div class="input-group relative flex items-stretch w-auto my-6">
      <input
        type="search"
        class="form-control flex-auto min-w-0 block w-full px-3 py-1.5 text-base font-normal text-gray-700 bg-white bg-clip-padding border-solid border-2 border-gray-400 rounded transition ease-in-out m-0 focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none"
        placeholder="🔍>Search"
        id="search"
        v-model="searchMail"
      />
    </div>

    <div class="grid grid-cols-1 gap-5 xl:flex">
      <div class="container border-solid rounded-xl p-1 shadow border-2 border-gray-400">
      <table
        class="min-w-full  block md:table rounded-xl"
      >
        <thead class="bg-white border-b block md:table-header-group">
          <tr class="border border-grey-500 md:border-none block md:table-row absolute -top-full md:top-auto -left-full md:left-auto  md:relative">
            <th
              scope="col"
              class="text-m border-x px-6 py-4 bg-gray-600 p-2 text-white font-bold md:border md:border-grey-500 text-left block md:table-cell"
              v-for="header in headers"
            >
              {{ header }}
            </th>
          </tr>
        </thead>
        <tbody class="block md:table-row-group">
          <tr
            class="bg-white border-b transition duration-300 ease-in-out hover:bg-blue-200 cursor-pointer md:border-none block md:table-row"
            v-for="mail in filteredMails"
            @click="getMail(mail)"
          >
            <td
              class="px-6 py-4 border-x whitespace-nowrap text-sm font-medium text-gray-900 md:border md:border-grey-500 text-left block md:table-cell"
            > <span class="inline-block w-1/3 md:hidden font-bold">Subject:</span>
              {{ mail.subject }}
            </td>
            <td
              class="px-6 py-4 border-x whitespace-nowrap text-sm font-medium text-gray-900 md:border md:border-grey-500 text-left block md:table-cell"
            > <span class="inline-block w-1/3 md:hidden font-bold">From:</span>
              {{ mail.from }}
            </td>
            <td
              class="px-6 py-4 border-x whitespace-nowrap text-sm font-medium text-gray-900 md:border md:border-grey-500 text-left block md:table-cell"
            > <span class="inline-block w-1/3 md:hidden font-bold">To:</span>
              {{ mail.to }}
            </td>
          </tr>
        </tbody>
      </table>
      </div>
      <div class="container w-full">
        <p
          class="text-base font-normal leading-relaxed text-gray-800 p-3 mb-3 shadow border-2 border-gray-400 rounded-xl"
        >
          <b>📧 | From: </b>{{ mailSelected.from }}<br />
          <b>📨 | To: </b>{{ mailSelected.to }}<br />
          <b>✒️ | Subject: </b>{{ mailSelected.subject }}
        </p>
        <p
          class="text-base font-normal leading-relaxed text-gray-800 p-3 shadow border-2 border-gray-400 rounded-xl text-justify"
        > 
          <b>🗒️ | Message:</b><br><br>
          {{ mailSelected.body }}
        </p>
      </div>
    </div>
  </div>
</template>
