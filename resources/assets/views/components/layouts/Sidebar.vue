<template>
  <div class="col-md-3 left_col">
    <div class="left_col scroll-view">
      <div class="navbar nav_title" style="border: 0;">
        <a href="index.html" class="site_title"><i class="fa fa-paw"></i> <span>[Accelerator]</span></a>
      </div>
      <div class="clearfix"></div>
      <!-- menu profile quick info -->
      <div class="profile">
        <div class="profile_pic">
          <img src="images/img.jpg" alt="..." class="img-circle profile_img">
        </div>
        <div class="profile_info">
          <span>Welcome,</span>
          <h2></h2>
        </div>
      </div>
      <!-- /menu profile quick info -->
      <!-- sidebar menu -->
      <div id="sidebar-menu" class="main_menu_side hidden-print main_menu">
        <div class="menu_section">
          <h3>General</h3>
          <ul class="nav side-menu">
            <li>
              <a <a v-link="'/'"><i class="fa fa-home"></i> Home <span class="fa fa-chevron-down"></span></a>
            </li>
            <li>
              <a><i class="fa fa-edit"></i> Marketplace Tool <span class="fa fa-chevron-down"></span></a>
              <ul class="nav child_menu">
                <li><a v-link="'price-overview'">Price Overview</a></li>
                <li><a v-link="'product-overview'">Product Overview</a></li>
                <li><a v-link="'upload-product'">Upload Product</a></li>
                <li><a v-link="'upload-marketplace-sku_mapping'">Upload Marketplace SKU Mapping</a></li>
                <li><a v-link="'tracer-sku-setting'">Tracer SKU Setting</a></li>
                <li><a v-link="'marketplace-content-export'" @click="resetMarketplaceId()">Marketplace Product Content Export</a></li>
                <li><a v-link="'marketplace-product-field-mapping'">Marketplace product field mapping</a></li>
                <li><a v-link="'update-sku-buyer'">SKU Buyer and Operator</a></li>
                <li><a v-link="'marketplace-courier-mapping'">Marketplace Courier Mappings</a></li>
                <li><a v-link="'order-settlement'">Marketplace Settlement - Exception List</a></li>
              </ul>
            </li>

            <li>
              <a><i class="fa fa-edit"></i> Reports <span class="fa fa-chevron-down"></span></a>
              <ul class="nav child_menu">
                <li><a v-link="'accelerator-sales-report'">Accelerator Sales report</a></li>
              </ul>
            </li>

            <li>
              <a><i class="fa fa-edit"></i> Compare Tools <span class="fa fa-chevron-down"></span></a>
              <ul class="nav child_menu">
                <li><a v-link="'freight-cost-compare-tool'">Freight Cost Compare Tool</a></li>
              </ul>
            </li>
          </ul>
        </div>
      </div>
      <!-- /sidebar menu -->
      <!-- /menu footer buttons -->
      <div class="sidebar-footer hidden-small">
        <a data-toggle="tooltip" data-placement="top" title="Settings">
          <span class="glyphicon glyphicon-cog" aria-hidden="true"></span>
        </a>
        <a data-toggle="tooltip" data-placement="top" title="FullScreen">
          <span class="glyphicon glyphicon-fullscreen" aria-hidden="true"></span>
        </a>
        <a data-toggle="tooltip" data-placement="top" title="Lock">
          <span class="glyphicon glyphicon-eye-close" aria-hidden="true"></span>
        </a>
        <a data-toggle="tooltip" data-placement="top" href="http://admincentre.eservicesgroup.com/auth/auth/deauth" title="Logout">
          <span class="glyphicon glyphicon-off" aria-hidden="true"></span>
        </a>
      </div>
      <!-- /menu footer buttons -->
    </div>
  </div>
</template>
<script>
  import {
    resetMarketplaceId,
  } from '../../../vuex/actions';
  export default {
    vuex: {
      actions: {
        resetMarketplaceId,
      }
    }
  }

  // Sidebar
  $(document).ready(function() {
    var CURRENT_URL = window.location.href.split('?')[0];
    // TODO: This is some kind of easy fix, maybe we can improve this
    var setContentHeight = function () {
      // reset height
      $('.right_col').css('min-height', $(window).height());

      var bodyHeight = $('body').outerHeight(),
        footerHeight = $('body').hasClass('footer_fixed') ? 0 : $('footer').height(),
        leftColHeight = $('.left_col').eq(1).height() + $('.sidebar-footer').height(),
        contentHeight = bodyHeight < leftColHeight ? leftColHeight : bodyHeight;

      // normalize content
      contentHeight -= $('.nav_menu').height() + footerHeight;

      // $('.right_col').css('min-height', contentHeight);
    };

    $('#sidebar-menu').find('a').on('click', function(ev) {
      var $li = $(this).parent();

      if ($li.is('.active')) {
        $li.removeClass('active active-sm');
        $('ul:first', $li).slideUp(function() {
          setContentHeight();
        });
      } else {
        // prevent closing menu if we are on child menu
        if ($li.parent().is('.child_menu')) {
          $('#sidebar-menu').find('li').removeClass('active active-sm');
          $('#sidebar-menu').find('li ul').slideUp();
        }

        $li.addClass('active');

        $('ul:first', $li).slideDown(function() {
          setContentHeight();
        });
      }
    });
     $('#sidebar-menu').find('ul ul').removeClass('active')
    // toggle small or large menu
    $('#menu_toggle').on('click', function() {
      if ($('body').hasClass('nav-md')) {
        $('#sidebar-menu').find('li.active ul').hide();
        $('#sidebar-menu').find('li.active').addClass('active-sm').removeClass('active');
      } else {
        $('#sidebar-menu').find('li.active-sm ul').show();
        $('#sidebar-menu').find('li.active-sm').addClass('active').removeClass('active-sm');
      }

      $('body').toggleClass('nav-md nav-sm');

      setContentHeight();
    });

    // recompute content when resizing
    $(window).smartresize(function(){
      setContentHeight();
    });

    setContentHeight();

    // fixed sidebar
    if ($.fn.mCustomScrollbar) {
      $('.menu_fixed').mCustomScrollbar({
        autoHideScrollbar: true,
        theme: 'minimal',
        mouseWheel:{ preventDefault: true }
      });
    }
  });
  // /Sidebar
</script>
