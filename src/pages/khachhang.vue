<template xmlns:v-slot="http://www.w3.org/1999/XSL/Transform">
  <q-layout>
    <q-page>
      <div id="q-app">
        <div class="">
          <div class="row">
            <div class="col-2">
              <div class="col-2" style="padding: 5px" >
                <div class="row" >
                  <q-item clickable v-ripple>
                    <q-item-section avatar>
                      <q-avatar>
                        <img src="https://cdn.quasar.dev/img/boy-avatar.png">
                      </q-avatar>
                    </q-item-section>
                    <q-item-section>Image avatar</q-item-section>
                  </q-item>
                </div>
                <div>
                  <q-input
                    class="customInput"
                    outlined
                    v-model="searchMa"
                    placeholder="Tìm kiếm theo mã"
                    :dense="true"
                  ></q-input>
                </div>
                <div>
                  <q-input
                    class="customInput"
                    outlined
                    v-model="searchTensdt"
                    placeholder="Tìm kiếm theo tên hoặc sdt"
                    :dense="true"
                  ></q-input>
                </div>
                <div>
                  <q-list>
                  <q-expansion-item
                    style="margin-top: 10px;background-color: darkgray;border-radius: 3px"
                    label="Tìm kiếm theo địa chỉ"
                    header-class="text-black"
                    expand-icon-class="text-black"
                  >
                    <q-card>
                      <q-card-section>
                        <q-input
                          class="customInput"
                          outlined
                          v-model="searchTensdt"
                          placeholder="Tìm theo tỉnh"
                          :dense="true"
                        ></q-input>
                        <q-input
                          class="customInput"
                          outlined
                          v-model="searchTensdt"
                          placeholder="Tìm theo thành phố"
                          :dense="true"
                        ></q-input>
                      </q-card-section>
                    </q-card>
                  </q-expansion-item>
                  </q-list>
                </div>
                <div>
                  <q-input
                    class="customInput"
                    outlined
                    v-model="searchtinhtrang"
                    placeholder="Tìm kiếm theo tình trạng"
                    :dense="true"
                  ></q-input>
                </div>
              </div>
            </div>
            <div class="col-10" style="padding: 5px">
              <div class="row">
                <p>QUẢN LÝ KHÁCH HÀNG</p>
                <q-space></q-space>
                <!--Button flex-end -->
                <div class="flex-container flex-end">
                  <q-btn
                    clickable
                    @click="dialog.add = true"
                    class="glossy q-ml-lg"
                    :dense="true"
                    color="primary"
                    icon="add"
                    label="Thêm khách hàng"
                  ></q-btn>
                  <q-btn
                    class="glossy q-ml-lg"
                    :dense="true"
                    color="primary"
                    icon="exit_to_app"
                    label="Import"
                  ></q-btn>
                  <q-btn
                    class="glossy q-ml-lg"
                    :dense="true"
                    color="primary"
                    icon="exit_to_app"
                    label="Xuất File"
                  ></q-btn>
                  <q-btn-dropdown
                    v-model="menu"
                    class="glossy q-ml-lg"
                    color="primary"
                    icon="apps"
                    :dense="true"
                  >
                    <div class="column" v-for="(item) in columns" v-bind:key="item.name">
                      <q-checkbox
                        right-label
                        :val="item.name"
                        v-model="visibleColumns"
                        :label="item.label"
                      ></q-checkbox>
                    </div>
                  </q-btn-dropdown>
                </div>
              </div>
              <div>
                <!--Table hàng hóa-->
                <div style="margin-top:20px">
                  <q-table
                    :visible-columns="visibleColumns"
                    :data="data"
                    :columns="columns"
                    row-key="name"
                    selection="multiple"
                    :selected.sync="selected"
                  >
                    <template v-slot:body="props">
                      <q-tr v-model="props.selected" @click.native="props.expand = !props.expand">
                        <q-td auto-width>
                          <q-checkbox v-model="props.selected"></q-checkbox>
                        </q-td>
                        <q-td key="makh" :props="props">{{ props.row.name }}</q-td>
                        <q-td key="tenkhachhang" :props="props">{{ props.row.tenkhachhang }}</q-td>
                        <q-td key="sdt" :props="props">{{ props.row.sdt }}</q-td>
                        <q-td key="email" :props="props">{{ props.row.email }}</q-td>
                        <q-td key="diachi" :props="props">{{ props.row.diachi }}</q-td>
                        <q-td key="khuvucquantam" :props="props">{{ props.row.khuvucquantam }}</q-td>
                        <q-td key="tinhtrang" :props="props">{{ props.row.tinhtrang }}</q-td>
                        <q-td key="nhucau" :props="props">{{ props.row.nhucau }}</q-td>
                        <q-td key="nguonnhan" :props="props">{{ props.row.nguonnhan }}</q-td>
                        <q-td key="ngaynhap" :props="props">
                         {{ props.row.ngaynhap }}
                        </q-td>
                        <q-td key="ngaysinh" :props="props">{{ props.row.ngaysinh }}</q-td>
                        <q-td key="manv" :props="props">
                          {{ props.row.manv }}
                        </q-td>
                        <q-td key="ghichu" :props="props">
                          {{ props.row.ghichu }}
                        </q-td>
                        <q-td key="tamtaichinh" :props="props">{{ props.row.tamtaichinh }}</q-td>
                      </q-tr>
                      <q-tr v-show="props.expand" :props="props">
                        <q-td colspan="100%" >
                          <div style="width: 1100px;background-color: aliceblue">
                            <div class="col-12 row">
                              <div class="col-4 pad">
                                <div class="col-12 row borderbot">
                                  <div class="col-4">
                                    <p>Mã khách hàng</p>
                                  </div>
                                  <div class="col-8">
                                    <p>{{props.row.name}}</p>
                                  </div>
                                </div>
                                <div class="col-12 row borderbot pad-chiild">
                                  <div class="col-4">
                                    <p>Tên Khách hàng</p>
                                  </div>
                                  <div class="col-8">
                                    <p>{{props.row.tenkhachhang}}</p>
                                  </div>
                                </div>
                                <div class="col-12 row borderbot pad-chiild">
                                  <div class="col-4">
                                    <p>Số điện thoại</p>
                                  </div>
                                  <div class="col-8">
                                    <p>{{props.row.sdt}}</p>
                                  </div>
                                </div>
                                <div class="col-12 row borderbot pad-chiild">
                                  <div class="col-4">
                                    <p>Email</p>
                                  </div>
                                  <div class="col-8">
                                    <p>{{props.row.email}}</p>
                                  </div>
                                </div>
                                <div class="col-12 row pad-chiild">
                                  <div class="col-4" style="background-color: azure">
                                    <p>Địa chỉ</p>
                                  </div>
                                  <div class="col-8">
                                    {{props.row.diachi}}
                                  </div>
                                </div>
                              </div>
                              <div class="col-4 pad">
                                <div class="col-12 row borderbot">
                                  <div class="col-4">
                                    <p>Khu vực quan tâm</p>
                                  </div>
                                  <div class="col-8">
                                    <p>{{props.row.khuvucquantam}}</p>
                                  </div>
                                </div>
                                <div class="col-12 row borderbot pad-chiild">
                                  <div class="col-4">
                                    <p>Tình trạng</p>
                                  </div>
                                  <div class="col-8">
                                    <p>{{props.row.tinhtrang}}</p>
                                  </div>
                                </div>
                                <div class="col-12 row borderbot pad-chiild">
                                  <div class="col-4">
                                    <p>Nhu cầu</p>
                                  </div>
                                  <div class="col-8">
                                    <p>{{props.row.nhucau}}</p>
                                  </div>
                                </div>
                                <div class="col-12 row borderbot pad-chiild">
                                  <div class="col-4">
                                    <p>Nguồn nhận</p>
                                  </div>
                                  <div class="col-8">
                                    <p>{{props.row.nguonnhan}}</p>
                                  </div>
                                </div>
                                <div class="col-12 row pad-chiild">
                                  <div class="col-4">
                                    <p>Ngày nhập</p>
                                  </div>
                                  <div class="col-8">
                                    <p>{{props.row.ngaynhap}}</p>
                                  </div>
                                </div>
                              </div>
                              <div class="col-4 pad">
                                <div class="col-12 row borderbot">
                                  <div class="col-4">
                                    <p>Ngày sinh</p>
                                  </div>
                                  <div class="col-8">
                                    <p>{{props.row.ngaysinh}}</p>
                                  </div>
                                </div>
                                <div class="col-12 row borderbot pad-chiild">
                                  <div class="col-4">
                                    <p>Mã Nhân viên</p>
                                  </div>
                                  <div class="col-8">
                                    <p>{{props.row.manv}}</p>
                                  </div>
                                </div>
                                <div class="col-12 row borderbot pad-chiild">
                                  <div class="col-4">
                                    <p>Tầm tài chính</p>
                                  </div>
                                  <div class="col-8">
                                    <p>{{props.row.tamtaichinh}}</p>
                                  </div>
                                </div>
                                <div class="col-12 row pad-chiild">
                                  <div class="col-4">
                                    <p>Ghi chú</p>
                                  </div>
                                  <div class="col-8">
                                    <p>{{props.row.ghichu}}</p>
                                  </div>
                                </div>
                              </div>
                            </div>
                            <div class="col-12" style="width: 1100px">
                              <div class="flex-container flex-end">
                                <q-btn
                                  class="glossy q-ml-lg"
                                  :dense="true"
                                  color="primary"
                                  icon="add"
                                  label="Lịch sử"
                                ></q-btn>
                                <q-btn
                                  v-on:click="editItem(props.row.__index)"
                                  class="glossy q-ml-lg"
                                  :dense="true"
                                  color="primary"
                                  icon="exit_to_app"
                                  label="Cập nhật"
                                ></q-btn>
                                <q-btn
                                  v-on:click="deleteItem(props.row.__index)"
                                  class="glossy q-ml-lg"
                                  :dense="true"
                                  color="primary"
                                  icon="exit_to_app"
                                  label="Xóa"
                                ></q-btn>
                              </div>
                            </div>
                          </div>
                        </q-td>
                      </q-tr>
                    </template>
                  </q-table>
                  <div class="q-mt-md">Selected: {{ JSON.stringify(selected) }}</div>
                </div>
                <!--Đóng table-->
                <!--Dialog thêm Khách hàng-->
                <q-dialog v-model="dialog.add" persistent>
                  <q-card class="col-12 " style="width: 1000px">
                    <q-card-section class="items-center">
                      <span class="col-12">Thêm Khách hàng</span>
                    </q-card-section>
                    <q-card-section>
                      <div class="q-gutter-y-md">
                        <q-tabs
                          v-model="tab"
                          dense
                          class="text-grey"
                          active-color="primary"
                          indicator-color="primary"
                          align="right"
                          narrow-indicator
                        >
                          <q-tab name="thongtins" label="Thông tin" no-caps></q-tab>
                        </q-tabs>
                        <q-separator></q-separator>
                        <!--Tab thông tin -->
                        <q-tab-panels v-model="tab">
                          <q-tab-panel name="thongtins" style="padding:0px">
                            <q-card style="width:100%">
                              <div class="row justify-between">
                                <div class="col-6" style="padding-right: 15px" >
                                  <!--Mã khách hàng-->
                                  <div class="row justify-between">
                                    <div class="self-center" style="width: 25%;">Mã khách hàng</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-input
                                        outlined
                                        v-model="addkhachhang.khachhang.name"
                                        placeholder="Mã hàng tự động"
                                        :dense="true"
                                      ></q-input>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Mã khách hàng là duy nhất</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                  <!--tên khách hàng-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Tên khách hàng</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-input outlined v-model="addkhachhang.khachhang.tenkhachhang" placeholder :dense="true"></q-input>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Tên của khách hàng</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                  <!--số điện thoại khách hàng-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Số điện thoại</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-input outlined v-model="addkhachhang.khachhang.sdt" type="tel" placeholder :dense="true"></q-input>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Số điện thoại của khách hàng</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                  <!--email khách hàng-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Email</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-input outlined v-model="addkhachhang.khachhang.email" placeholder :dense="true"></q-input>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Email của khách hàng</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                  <!--Ngày sinh-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Ngày sinh</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-input outlined v-model="addkhachhang.khachhang.ngaysinh" mask="date" :rules="['date']" :dense="true">
                                        <template v-slot:append>
                                          <q-icon name="event" class="cursor-pointer">
                                            <q-popup-proxy ref="qDateProxy" transition-show="scale" transition-hide="scale">
                                              <q-date v-model="ngaysinh" @input="() => $refs.qDateProxy.hide()" ></q-date>
                                            </q-popup-proxy>
                                          </q-icon>
                                        </template>
                                      </q-input>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Nguồn nhập của khách hàng</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                  <!--Địa chỉ-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Địa chỉ</div>
                                    <div class="self-center border-address" style="width: 67%;">
                                      <q-input outlined v-model="addkhachhang.khachhang.diachi" placeholder :dense="true"></q-input>
                                      <q-select class="marTop" outlined v-model="modelxa" :options="optionsXa" :dense="true" label="Phường/Xã" >
                                      </q-select>
                                      <q-select class="marTop" outlined v-model="modelhuyen" :options="optionsHuyen" :dense="true" label="Quận/Huyện" >
                                      </q-select>
                                      <q-select class="marTop" outlined v-model="modelthanhpho" :options="optionsTP" :dense="true" label="Tỉnh/Thành Phố" >
                                      </q-select>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Ngày sinh của khách hàng</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                  <!--Khu vực quan tâm-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Khu vực quan tâm</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-input outlined v-model="addkhachhang.khachhang.khuvucquantam" placeholder :dense="true"></q-input>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Ngày sinh của khách hàng</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                </div>
                                <div class="col-6" style="padding-left: 15px" >
                                  <!--Tình trạng-->
                                  <div class="row justify-between">
                                    <div class="self-center" style="width: 25%;">Tình trạng</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-select
                                        outlined
                                        v-model="addkhachhang.khachhang.tinhtrang"
                                        :options="optionsTinhtrang"
                                        :dense="true"
                                      ></q-select>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Lựa chọn nhóm hàng cho sản phẩm</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                  <!--Nhu cầu-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Nhu cầu</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-input outlined v-model="addkhachhang.khachhang.nhucau" placeholder :dense="true"></q-input>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Nhu cầu khách hàng</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                  <!--Nguồn nhập-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Nguồn nhập</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-input outlined v-model="addkhachhang.khachhang.nguonnhan" placeholder :dense="true"></q-input>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Nguồn nhập của khách hàng</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                  <!--Ngày nhập-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Ngày nhập</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-input outlined v-model="addkhachhang.khachhang.ngaynhap" mask="date" :rules="['date']" :dense="true">
                                        <template v-slot:append>
                                          <q-icon name="event" class="cursor-pointer">
                                            <q-popup-proxy ref="qDateProxy" transition-show="scale" transition-hide="scale">
                                              <q-date v-model="ngaynhap" @input="() => $refs.qDateProxy.hide()" ></q-date>
                                            </q-popup-proxy>
                                          </q-icon>
                                        </template>
                                      </q-input>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Nguồn nhập của khách hàng</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                  <!--Mã nhân viên-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Mã nhân viên</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-input outlined v-model="addkhachhang.khachhang.manv" placeholder :dense="true"></q-input>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Mã của nhân viên</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                  <!--Ghi chú-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Ghi chú</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-input outlined v-model="addkhachhang.khachhang.ghichu" type="textarea" placeholder :dense="true"></q-input>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Nguồn nhập của khách hàng</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                  <!--Tầm tài chính-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Tầm tài chính</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-select
                                        outlined
                                        v-model="addkhachhang.khachhang.tamtaichinh"
                                        :options="optionsTamtaichinh"
                                        :dense="true"
                                      ></q-select>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Mã của nhân viên</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                </div>
                              </div>
                            </q-card>
                          </q-tab-panel>
                          <!--Đóng tab thông tin-->
                        </q-tab-panels>
                        <div class="col-md-12 col-12 col-sm-12">
                          <q-card-actions align="right">
                            <q-btn
                              style="margin-top:10px"
                              class="col-md-2 col-12 col-sm-12"
                              icon="save"
                              label="Thêm mới"
                              color="primary"
                              v-on:click="ADDKHACHHANG()"
                              no-caps
                            ></q-btn>
                            <q-btn
                              style="margin-top:10px"
                              class="col-md-2 col-12 col-sm-12"
                              icon="cancel"
                              label="Hủy"
                              color="grey"
                              @click="dialog.add = false"
                              no-caps
                            ></q-btn>
                          </q-card-actions>
                        </div>
                      </div>
                    </q-card-section>
                  </q-card>
                </q-dialog>
                <!-- Hết Dialog thêm hàng hóa-->
                <!--/////////////////////////////////////////////////////////////////////////////////////////////////////-->
                <!--Dialog Cập nhật khách hàng-->
                <q-dialog v-model="dialog.edit" persistent>
                  <q-card class="col-12 " style="width: 1000px">
                    <q-card-section class="items-center">
                      <span class="col-12">Cập nhật khách hàng</span>
                    </q-card-section>
                    <q-card-section>
                      <div class="q-gutter-y-md">
                        <q-tabs
                          v-model="tab"
                          dense
                          class="text-grey"
                          active-color="primary"
                          indicator-color="primary"
                          align="right"
                          narrow-indicator
                        >
                          <q-tab name="thongtins" label="Thông tin" no-caps></q-tab>
                        </q-tabs>
                        <q-separator></q-separator>
                        <!--Tab thông tin -->
                        <q-tab-panels v-model="tab">
                          <q-tab-panel name="thongtins" style="padding:0px">
                            <q-card style="width:100%">
                              <div class="row justify-between">
                                <div class="col-6" style="padding-right: 15px" >
                                  <!--tên khách hàng-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Tên khách hàng</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-input outlined v-model="editkhachhang.khachhang.tenkhachhang" placeholder :dense="true"></q-input>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Tên của khách hàng</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                  <!--số điện thoại khách hàng-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Số điện thoại</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-input outlined v-model="editkhachhang.khachhang.sdt" type="tel" placeholder :dense="true"></q-input>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Số điện thoại của khách hàng</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                  <!--email khách hàng-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Email</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-input outlined v-model="editkhachhang.khachhang.email" placeholder :dense="true"></q-input>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Email của khách hàng</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                  <!--Ngày sinh-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Ngày sinh</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-input outlined v-model="editkhachhang.khachhang.ngaysinh" mask="date" :rules="['date']" :dense="true">
                                        <template v-slot:append>
                                          <q-icon name="event" class="cursor-pointer">
                                            <q-popup-proxy ref="qDateProxy" transition-show="scale" transition-hide="scale">
                                              <q-date v-model="ngaysinh" @input="() => $refs.qDateProxy.hide()" ></q-date>
                                            </q-popup-proxy>
                                          </q-icon>
                                        </template>
                                      </q-input>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Nguồn nhập của khách hàng</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                  <!--Địa chỉ-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Địa chỉ</div>
                                    <div class="self-center border-address" style="width: 67%;">
                                      <q-input outlined v-model="editkhachhang.khachhang.diachi" placeholder :dense="true"></q-input>
                                      <q-select class="marTop" outlined v-model="modelxa" :options="optionsXa" :dense="true" label="Phường/Xã" >
                                      </q-select>
                                      <q-select class="marTop" outlined v-model="modelhuyen" :options="optionsHuyen" :dense="true" label="Quận/Huyện" >
                                      </q-select>
                                      <q-select class="marTop" outlined v-model="modelthanhpho" :options="optionsTP" :dense="true" label="Tỉnh/Thành Phố" >
                                      </q-select>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Ngày sinh của khách hàng</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                  <!--Khu vực quan tâm-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Khu vực quan tâm</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-input outlined v-model="editkhachhang.khachhang.khuvucquantam" placeholder :dense="true"></q-input>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Ngày sinh của khách hàng</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                </div>
                                <div class="col-6" style="padding-left: 15px" >
                                  <!--Tình trạng-->
                                  <div class="row justify-between">
                                    <div class="self-center" style="width: 25%;">Tình trạng</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-select
                                        outlined
                                        v-model="editkhachhang.khachhang.tinhtrang"
                                        :options="optionsTinhtrang"
                                        :dense="true"
                                      ></q-select>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Lựa chọn nhóm hàng cho sản phẩm</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                  <!--Nhu cầu-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Nhu cầu</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-input outlined v-model="editkhachhang.khachhang.nhucau" placeholder :dense="true"></q-input>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Nhu cầu khách hàng</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                  <!--Nguồn nhập-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Nguồn nhập</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-input outlined v-model="editkhachhang.khachhang.nguonnhan" placeholder :dense="true"></q-input>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Nguồn nhập của khách hàng</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                  <!--Ngày nhập-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Ngày nhập</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-input outlined v-model="editkhachhang.khachhang.ngaynhap" mask="date" :rules="['date']" :dense="true">
                                        <template v-slot:append>
                                          <q-icon name="event" class="cursor-pointer">
                                            <q-popup-proxy ref="qDateProxy" transition-show="scale" transition-hide="scale">
                                              <q-date v-model="ngaynhap" @input="() => $refs.qDateProxy.hide()" ></q-date>
                                            </q-popup-proxy>
                                          </q-icon>
                                        </template>
                                      </q-input>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Nguồn nhập của khách hàng</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                  <!--Mã nhân viên-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Mã nhân viên</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-input outlined v-model="editkhachhang.khachhang.manv" placeholder :dense="true"></q-input>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Mã của nhân viên</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                  <!--Ghi chú-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Ghi chú</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-input outlined v-model="editkhachhang.khachhang.ghichu" type="textarea" placeholder :dense="true"></q-input>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Nguồn nhập của khách hàng</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                  <!--Tầm tài chính-->
                                  <div class="row justify-between marTop">
                                    <div class="self-center" style="width: 25%;">Tầm tài chính</div>
                                    <div class="self-center" style="width: 67%;">
                                      <q-select
                                        outlined
                                        v-model="editkhachhang.khachhang.tamtaichinh"
                                        :options="optionsTamtaichinh"
                                        :dense="true"
                                      ></q-select>
                                    </div>
                                    <div class="self-center" style="width: 5%;">
                                      <q-icon
                                        name="notification_important"
                                        style="color: #777777; font-size: 1.4em;"
                                      >
                                        <q-tooltip
                                          anchor="center right"
                                          self="center left"
                                          :offset="[10, 10]"
                                        >Mã của nhân viên</q-tooltip>
                                      </q-icon>
                                    </div>
                                  </div>
                                </div>
                              </div>
                            </q-card>
                          </q-tab-panel>
                          <!--Đóng tab thông tin-->
                        </q-tab-panels>
                        <div class="col-md-12 col-12 col-sm-12">
                          <q-card-actions align="right">
                            <q-btn
                              style="margin-top:10px"
                              class="col-md-2 col-12 col-sm-12"
                              icon="save"
                              label="Cập nhật"
                              v-on:click="EDITKHACHHANG()"
                              color="green"
                              no-caps
                            ></q-btn>
                            <q-btn
                              style="margin-top:10px"
                              class="col-md-2 col-12 col-sm-12"
                              icon="cancel"
                              label="Hủy"
                              color="grey"
                              @click="dialog.edit = false"
                              no-caps
                            ></q-btn>
                          </q-card-actions>
                        </div>
                      </div>
                    </q-card-section>
                  </q-card>
                </q-dialog>
                <!--Hết dialog cập nhật khách hàng-->
                <!--/////////////////////////////////////////////////////////////////////////////////////////////////////-->
                <!-- dialog xóa khách hàng-->
<!--                <d-dialogDeletes :dialog="dialog.remove" @closeDialogs="dialog.remove = false" title="Please confirm" content="Are you sure you want to delete this field?">-->
<!--                  <q-btn dense no-caps outline color="red" v-on:click="dialog.remove = false" label="Cancel" class="format-buttons"></q-btn>-->
<!--                  <q-btn dense no-caps outline color="primary" label="Confirm" v-on:click="deleteKhachHang()"  class="format-buttons"></q-btn>-->
<!--                </d-dialogDeletes>-->
                <q-dialog v-model="dialog.remove" persistent>
                  <q-card>
                    <q-card-section class="row">
                      <q-avatar icon="delete_forever" text-color="primary" ></q-avatar>
                      <span class="q-ml-sm row" style="margin-top: 15px">You are currently not connected to any network.</span>
                    </q-card-section>

                    <q-card-actions align="right">
                      <q-btn flat label="Cancel" v-on:click="dialog.remove = false" color="primary"  ></q-btn>
                      <q-btn flat label="Delete" color="primary" v-on:click="deleteKhachHang()" ></q-btn>
                    </q-card-actions>
                  </q-card>
                </q-dialog>
                <!-- Hết dialog xóa khách hàng -->
              </div>
            </div>
          </div>
        </div>
      </div>
    </q-page>
  </q-layout>
</template>
<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      searchMa: '',
      searchTensdt: '',
      searchtinhtrang: '',
      dialogThemKhachhang: false,
      tab: 'thongtins',
      menu: false,
      props: ['columns', 'data', 'selected', 'visibleColumns'],
      selected: [],
      model: null,
      modelxa: null,
      modelhuyen: null,
      modelthanhpho: null,
      modelTinhtrang: null,
      modelTamtaichinh: null,
      ngaynhap: '2019/02/01',
      ngaysinh: '1965/07/14',
      dialog: { edit: false, remove: false, add: false },
      tel: '',
      optionsXa: ['Eayong', 'Phường Tân Lợi'],
      optionsHuyen: ['KrongPack', 'Huyện Lăk', 'tp.Buôn Ma thuột'],
      optionsTP: ['DakLak', 'Lâm đồng'],
      optionsTinhtrang: ['Lead', 'KHPS', 'KH Tiềm Năng', 'Thuê Bao'],
      optionsTamtaichinh: ['500 triệu - 700 triệu', '700 triệu - 1 tỷ', '1 tỷ - 1,5 tỷ', '1,5 tỷ trở lên'],
      addkhachhang: {
        khachhang: { name: 3,
          tenkhachhang: '',
          sdt: '',
          email: '',
          diachi: '',
          khuvucquantam: '',
          tinhtrang: '',
          nhucau: '',
          nguonnhan: '',
          ngaynhap: '',
          ngaysinh: '',
          manv: '',
          ghichu: '',
          tamtaichinh: ''
        }
      },
      editkhachhang: {
        khachhang: { name: 3,
          tenkhachhang: '',
          sdt: '',
          email: '',
          diachi: '',
          khuvucquantam: '',
          tinhtrang: '',
          nhucau: '',
          nguonnhan: '',
          ngaynhap: '',
          ngaysinh: '',
          manv: '',
          ghichu: '',
          tamtaichinh: ''
        }
      },
      columns: [
        {
          name: 'makh',
          required: true,
          label: 'Mã Khách hàng',
          align: 'left',
          field: row => row.name,
          format: val => `${val}`,
          sortable: true
        },
        {
          name: 'tenkhachhang',
          align: 'center',
          label: 'Tên khách hàng',
          field: 'tenkhachhang',
          sortable: true
        },
        { name: 'sdt', label: 'Số điện thoại', field: 'sdt', sortable: true },
        { name: 'email', label: 'Email', field: 'email' },
        { name: 'diachi', label: 'Địa chỉ', field: 'diachi' },
        { name: 'khuvucquantam', label: 'Khu vực quan tâm', field: 'khuvucquantam' },
        { name: 'tinhtrang', label: 'Tình trạng', field: 'tinhtrang' },
        { name: 'nhucau', label: 'Nhu cầu', field: 'nhucau' },
        {
          name: 'nguonnhan',
          label: 'Nguồn Nhận',
          field: 'nguonnhan'
        },
        {
          name: 'ngaynhap',
          label: 'Ngày nhập',
          field: 'ngaynhap'
        },
        { name: 'ngaysinh', label: 'Ngày sinh', field: 'ngaysinh' },
        {
          name: 'manv',
          label: 'Mã nhân viên',
          field: 'manv'
        },
        {
          name: 'ghichu',
          label: 'Ghi chú',
          field: 'ghichu'
        },
        { name: 'tamtaichinh', label: 'Tầm tài chính', field: 'tamtaichinh' }
      ],
      data: [
        {
          name: 'KH01',
          tenkhachhang: 'Nguyễn Văn A',
          sdt: 9897655200,
          email: 'nguyenvana@gmail.com',
          diachi: '123-Nguyễn Huệ-Phường Tân Lập-tp.Buôn Ma Thuột',
          khuvucquantam: 'Khu đô thị',
          tinhtrang: 'Gọi lần 2',
          nhucau: 'Mua nhà ở',
          nguonnhan: 'Auto call',
          ngaynhap: '1/2/2013',
          ngaysinh: '12/12/1967',
          manv: 'NV01',
          ghichu: 'Ở gần trung tâm',
          tamtaichinh: '500tr-1tỷ'
        },
        {
          name: 'KH02',
          tenkhachhang: 'Nguyễn Văn B',
          sdt: 9897655200,
          email: 'nguyenvanB@gmail.com',
          diachi: '123-Nguyễn Huệ-Phường Tân Lập-tp.Buôn Ma Thuột',
          khuvucquantam: 'Khu đô thị',
          tinhtrang: 'Gọi lần 2',
          nhucau: 'Mua nhà ở',
          nguonnhan: 'Auto call',
          ngaynhap: '1/2/2013',
          ngaysinh: '12/12/1967',
          manv: 'NV01',
          ghichu: 'Ở gần trung tâm',
          tamtaichinh: '500tr-1tỷ'
        },
        {
          name: 'KH03',
          tenkhachhang: 'Nguyễn Văn An',
          sdt: 9877988547,
          email: 'nguyenvanC@gmail.com',
          diachi: '123-Nguyễn Huệ-Phường Tân Lập-tp.Buôn Ma Thuột',
          khuvucquantam: 'Khu đô thị',
          tinhtrang: 'Gọi lần 2',
          nhucau: 'Mua nhà ở',
          nguonnhan: 'Auto call',
          ngaynhap: '1/2/2013',
          ngaysinh: '12/12/1967',
          manv: 'NV01',
          ghichu: 'Ở gần trung tâm',
          tamtaichinh: '500tr-1tỷ'
        }
      ],
      visibleColumns: [
        'name',
        'tenkhachhang',
        'sdt',
        'diachi',
        'tinhtrang',
        'nguonnhan',
        'ngaynhap',
        'manv',
        'tamtaichinh'
      ]
    }
  },
  methods: {
    ADDKHACHHANG () {
      this.addkhachhang.khachhang.name = this.addkhachhang.khachhang.name + 1
      this.data.push({
        name: this.addkhachhang.khachhang.name,
        tenkhachhang: this.addkhachhang.khachhang.tenkhachhang,
        sdt: this.addkhachhang.khachhang.sdt,
        email: this.addkhachhang.khachhang.email,
        diachi: this.addkhachhang.khachhang.diachi,
        khuvucquantam: this.addkhachhang.khachhang.khuvucquantam,
        tinhtrang: this.addkhachhang.khachhang.tinhtrang,
        nhucau: this.addkhachhang.khachhang.nhucau,
        nguonnhap: this.addkhachhang.khachhang.nguonnhap,
        ngaynhap: this.addkhachhang.khachhang.ngaynhap,
        ngaysinh: this.addkhachhang.khachhang.ngaysinh,
        manv: this.addkhachhang.khachhang.manv,
        ghichu: this.addkhachhang.khachhang.ghichu,
        tamtaichinh: this.addkhachhang.khachhang.tamtaichinh })
      this.dialog.add = false
    },
    editItem (index) {
      this.index = index
      this.editkhachhang.khachhang.tenkhachhang = this.data[index].tenkhachhang
      this.editkhachhang.khachhang.sdt = this.data[index].sdt
      this.editkhachhang.khachhang.email = this.data[index].email
      this.editkhachhang.khachhang.diachi = this.data[index].diachi
      this.editkhachhang.khachhang.khuvucquantam = this.data[index].khuvucquantam
      this.editkhachhang.khachhang.tinhtrang = this.data[index].tinhtrang
      this.editkhachhang.khachhang.nhucau = this.data[index].nhucau
      this.editkhachhang.khachhang.nguonnhan = this.data[index].nguonnhan
      this.editkhachhang.khachhang.ngaynhap = this.data[index].ngaynhap
      this.editkhachhang.khachhang.ngaysinh = this.data[index].ngaysinh
      this.editkhachhang.khachhang.manv = this.data[index].manv
      this.editkhachhang.khachhang.ghichu = this.data[index].ghichu
      this.editkhachhang.khachhang.tamtaichinh = this.data[index].tamtaichinh
      this.dialog.edit = true
    },
    EDITKHACHHANG () {
      this.data[this.index].tenkhachhang = this.editkhachhang.khachhang.tenkhachhang
      this.data[this.index].sdt = this.editkhachhang.khachhang.sdt
      this.data[this.index].email = this.editkhachhang.khachhang.email
      this.data[this.index].diachi = this.editkhachhang.khachhang.diachi
      this.data[this.index].khuvucquantam = this.editkhachhang.khachhang.khuvucquantam
      this.data[this.index].tinhtrang = this.editkhachhang.khachhang.tinhtrang
      this.data[this.index].nhucau = this.editkhachhang.khachhang.nhucau
      this.data[this.index].nguonnhan = this.editkhachhang.khachhang.nguonnhan
      this.data[this.index].ngaynhap = this.editkhachhang.khachhang.ngaynhap
      this.data[this.index].ngaysinh = this.editkhachhang.khachhang.ngaysinh
      this.data[this.index].manv = this.editkhachhang.khachhang.manv
      this.data[this.index].ghichu = this.editkhachhang.khachhang.ghichu
      this.data[this.index].tamtaichinh = this.editkhachhang.khachhang.tamtaichinh
      this.dialog.edit = false
    },
    deleteItem (index) {
      this.index = index
      this.dialog.remove = true
    },
    deleteKhachHang () {
      this.data.splice(this.index, 1)
      this.dialog.remove = false
    }
  }
}
</script>
<style>
  .row{
    padding: 0px;
  }
  .customInput {
    margin-top: 10px;
    background-color: rgb(255, 255, 255);
  }
  .flex-end {
    justify-content: flex-end;
  }
  .flex-container {
    padding: 0;
    margin: 0;
    list-style: none;
    display: flex;
  }
  .borderbot{
    border-bottom: 1px solid gray;
  }
  .border-address{
    border: 1px solid darkgray;
    border-radius: 5px;
    padding: 5px;
  }
  .pad{
    padding: 15px;
  }
  .pad-chiild{
    padding-top: 15px;
  }
  .q-tabs__content--align-justify .q-tab {
    flex: 0 0 auto;
  }
  .q-tabs__content {
    overflow: hidden;
     flex: 0 0 auto;
  }
  .marTop {
    margin-top: 6px;
  }
</style>
