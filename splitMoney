<?php
namespace app\services\splitmoney;

use Exception;

/**
 * 分账 主要处理商家货款
 * Class SplitMoneyServices
 * @package app\services\Splitmoney
 * @method array getShopInfoArray(array $where, string $field, string $key) 根据条件查询对应的商家信息以数组形式返回
 * 实例化使用 \app\services\Splitmoney\SharingMoneyServices::getInstance()
 * 分账商家货款  \app\services\Splitmoney\SharingMoneyServices::getInstance()::setShopInfo()::byOrder('订单备注');
 * 设置订单备注 \app\services\Splitmoney\SharingMoneyServices::getInstance()::setShopId(商家id)
 */
class SplitMoneyServices
{

    //创建静态私有的变量保存该类对象
    static private $instance;
    
    /* @var mixed */
    protected  $StoreOrderDao;
    
    /* @var object 订单详情 */
    public  $orderInfo   = null;
    
    /* @var string|float 手续费*/
    public $charge = 0;
    
    /* @var string 系统订单id */
    public $sys_order_id = null;
    
    /* @var string 第三方订单编号 */
    public $trade_no     = null;

    /* @var object 用户详情 */
    public  $userInfo    = null;
    /* @var int 用户id */
    public  $uid         = 0;

    /* @var string 名称 */
    public $name         = '缺省';
    /**
     * 分账账号
     * @var null
     */
    public $account     = null;
    /**
     * 分账接收方列表
     * @var array | null
     */
    public $sharingInfo = null;

    /* @var object 商家详情 */
    public  $shopInfo    = null;

    /* @var string 类型明细 */
    public $sharing_type = 'wait';

    /**
     * SharingMoneyServices constructor.
     */
    public function __construct()
    {
        /**
         * @param StoreOrderDao $StoreOrderDao
         */
        $this->StoreOrderDao      = app()->make(StoreOrderDao::class);
        /**
    }
    //防止使用clone克隆对象
    private function __clone(){}
    public static function getInstance()
    {
        //判断$instance是否是Singleton的对象，不是则创建
        if (!self::$instance instanceof self) {
            self::$instance = new SplitMoneyServices();
        }
        return self::$instance;
    }

    /**
     * 设置本次分账金额;单位分
     * @param $money
     * @return void
     */
    public static function setMoney(int $money = 0){
        self::$instance->money = $money * 100 ?? 0 ;
        return self::$instance;
    }

    /**
     * 获取本次分账金额;单位分
     * @return mixed
     */
    public static function getMoney(){
        return self::$instance->money;
    }
    /**
     * 设置手续费
     * @param $charge
     * @return mixed
     */
    public static function setCharge($charge){
        self::$instance->charge = $charge;
        return self::$instance;
    }

    /**
     * 获取手续费
     * @return mixed
     */
    public static function getCharge(){
        return self::$instance->charge;
    }
    /**
     * 设置订单id
     * @param $oid int 订单id
     * @return mixed
     */
    public static function setOid($oid){
        self::$instance->oid = $oid;
        return self::$instance;
    }

    /**
     * 获取订单id
     * @return mixed
     */
    public static function getOid(){
        return self::$instance->oid;
    }
    /**
     * 获取订单数组
     * @return mixed |array
     */
    public static function getOrderInfoToArray($return = 'array')
    {
        if($return == 'array'){
            return self::$instance->orderInfo->toArray();
        }else{
            return self::$instance->orderInfo;
        }
    }
    /**
     * 设置用户Uid
     * @param $uid
     * @return mixed
     */
    public static function setUid($uid){
        self::$instance->uid = $uid;
        return self::$instance;
    }

    /**
     * 获取用户id
     * @return mixed
     */
    public static function getUid(){
        return self::$instance->uid;
    }

    /**
     * 获取商家id
     * @return mixed
     */
    public static function getShopId(){
        return self::$instance->shop_id;
    }

    /**
     * 设置分账账号
     * @param $account
     * @return null
     */
    public static function setAccount($account){
        self::$instance->account = $account ?? null;
        return self::$instance;
    }
    /**
     * 获取分账账号
     * @return mixed
     */
    public static function getAccount(){
        return self::$instance->account;
    }
    /* 设置分账明细类型
    * @param $account
    * @return null
    */
    public static function setType($type){
        self::$instance->type = $type ?? null;
        return self::$instance;
    }
    /**
     * 获取设置分账明细类型
     * @return mixed
     */
    public static function getType(){
        return self::$instance->type;
    }
    /* 设置分账结算类型
    * @param $account
    * @return null
    */
    public static function setSplitType($type){
        self::$instance->split_type = $type ?? null;
        return self::$instance;
    }
    /**
     * 获取设置分账结算类型
     * @return mixed
     */
    public static function getSplitType(){
        return self::$instance->split_type;
    }

    /**
     * 设置分账接收方信息 二维数组
     * 微信[[接收方1][接收方2]]
     * @param $array
     * @return mixed
     */
    public static function setSharingInfoToArray($array = []){
        if(self::$instance->sharingInfo !== null && !empty($array)){
            self::$instance->sharingInfo = array_merge(self::$instance->sharingInfo , $array);
        }else{
            self::$instance->sharingInfo = $array;
        }

        return self::$instance;
    }

    /**
     * 获取分账接收方信息
     * @return mixed
     */
    public static function getSharingInfoToArray(){
        return self::$instance->sharingInfo;
    }

    /**
     * 设置关联ID
     * @param $link_id
     * @return mixed
     */
    public static function setLinkId($link_id){
        self::$instance->link_id = $link_id;
        return self::$instance;
    }

    /**
     * 获取关联ID
     * @return mixed
     */
    public static function getLinkId(){
        return self::$instance->link_id;
    }
    /**
     * 设置名称
     * @param $link_id
     * @return mixed
     */
    public static function setName($name){
        self::$instance->name = $name;
        return self::$instance;
    }

    /**
     * 获取名称
     * @return mixed
     */
    public static function getName(){
        return self::$instance->name;
    }
    /**
     * 设置剩余
     * @param $link_id
     * @return mixed
     */
    public static function setBalance($balance){
        self::$instance->balance = $balance;
        return self::$instance;
    }
    /**
     * 获取剩余
     * @return mixed
     */
    public static function getBalance(){
        return self::$instance->balance;
    }

    /**
     * 设置分账收益类型
     * @param $pm  1=获得 0=支出
     * @return mixed
     */
    public static function setPm($pm){
        self::$instance->pm = $pm;
        return self::$instance;
    }

    /**
     * 获取分账类型
     * @return mixed
     */
    public static function getPm(){
        return self::$instance->pm;
    }

    /**
     * 设置备注
     * @param $mark
     * @return void
     */
    public static function setMark($mark){
        self::$instance->mark = $mark;
        return self::$instance;
    }

    /**
     * 获取备注
     * @return mixed
     */
    public static function getMark(){
        return self::$instance->mark;
    }

    /**
     * 设置状态
     * @param $status 0=待确定 1=有效 -1=无效
     * @return void
     */
    public static function setStatus($status){
        self::$instance->status = $status;
        return self::$instance;
    }

    /**
     * 获取状态
     * @return mixed
     */
    public static function getStatus(){
        return self::$instance->status;
    }

    /**
     * 设置是否收货
     * @param $take
     * @return mixed
     */
    public static function setTake($take){
        self::$instance->take = $take;
        return self::$instance;
    }

    /**
     * 获取是否收货
     * @return mixed
     */
    public static function getTake(){
        return self::$instance->take;
    }

    /**
     * 获取冻结到期时间
     * @return mixed
     */
    public static function getFrozenTime(){
        return self::$instance->frozen_time;
    }
    /**
     * 获取第三方交易单号
     * @return mixed
     */
    public static function getTradeNo(){
        return self::$instance->trade_no;
    }
    /**
     * 返回请求第三方成功时间
     * @return mixed
     */
    public static function getSuccessTime(){
        return self::$instance->success_time;
    }

    /**
     * 设置执行时间
     * @param $implement_time
     * @return mixed
     */
    public static function setImplementTime($implement_time){
        self::$instance->implement_time = $implement_time;
        return self::$instance;
    }
    /**
     * 设置系统订单id
     * @param $order_id
     * @return mixed
     */
    public static function setSysOrderId($order_id){
        self::$instance->sys_order_id = $order_id;
        return self::$instance;
    }
    /**
     * 设置回调信息
     * @param $result_info
     * @return mixed
     */
    public static function setResultInfo($result_info = []){
        if(is_array($result_info)){
            $result_info = json_encode($result_info,true);
        }
        self::$instance->result_info = $result_info;
        return self::$instance;
    }
    /**
     * 设置第三方订单号
     * @param $transaction_id
     * @return mixed
     */
    public static function setTransactionId($transaction_id){
        self::$instance->transaction_id = $transaction_id;
        return self::$instance;
    }
    /**
     * 设置分账订单号
     * @param $code
     * @return void
     */
    public static function setSharingOrderCode($code){
        self::$instance->order_code = $code;
        return self::$instance;
    }

    /**
     * 获取分账订单号
     * @return mixed
     */
    public static function getSharingOrderCode(){
        return self::$instance->order_code;
    }

    /**
     * 设置场景:unknown=未知;online=线上;offline=线下
     * @param $scene
     * @return mixed
     */
    public static function setScene($scene = 'online'){
        self::$instance->scene = $scene;
        return self::$instance;
    }

    /**
     * 获取场景
     * @return mixed
     */
    public static function getScene(){
        return self::$instance->scene;
    }
    /**
     * 分帐到微信
     * @param string $remarks 备注
     * @param int $single 1= 单次分账；2多次分账
     * @return mixed
     * @throws Exception
     */
    public static function sharingWeixin($remarks = '',$single = 2){
        if(self::$instance->account == null || self::$instance->sharingInfo == null){
            throw new Exception('请设置分账账户不存在');
        }
        if(self::$instance->trade_no == null && self::$instance->sys_order_id == null){
            throw new Exception('第三方订单号和系统订单号不能为空');
        }
        //请求微信分账
        $result = self::profitsharingRequest($remarks,$single);
        if(!$result){
            throw new Exception('请求微信分账失败');
        }
        self::$instance::setResultInfo($result);
        return self::$instance;
    }
}
