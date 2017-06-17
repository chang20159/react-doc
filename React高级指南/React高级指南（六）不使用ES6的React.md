## merchant-member

API:
pom:

    <dependency>
        <groupId>com.dianping</groupId>
        <artifactId>merchant-member-api</artifactId>
        <version>2.0.6-SNAPSHOT</version>
    </dependency>

UserAuthoriseRPCService:

    /**
     * 获取账号对应的授权，包括门店绑定的授权
     * @param userId
     * @return
     * @throws ShopAccountNotFoundException
     */
    List<AuthorizationDetailInfoDTO> queryDetailAuthorizationInfo(int userId) throws ShopAccountNotFoundException;
    
    /**
     * 获取账号在某个customer的某个shop下面有什么权限
     * @param userId
     * @param customerId
     * @param shopId
     * @return
     */
    List<FunctionDTO> queryAuthorisedFunctions(int userId, int customerId, int shopId);
    
    List<Integer> queryAuthorisedFuncIds(int userId, int customerId, int shopId);
    
    /**
     * 用户是否是为管理员
     * @param userId
     * @return
     * @throws ShopAccountNotFoundException
     */
    boolean isUserAdmin(int userId) throws  ShopAccountNotFoundException;
    
    /**
     * 通过CustomerId获取管理员账号
     * @param customerId
     * @return
     */
    ShopAccountDTO getCustomerAdminUser(int customerId);
    
    /**
     * 批量根据CustomerId查询管理员账号
     * @param customerIds
     * @return
     */
    List<ShopAccountDTO> getAdminUsersByCustomerIds(List<Integer> customerIds);
    
    /**
     * 根据账号查询门店
     * @param shopAccountId
     * @return
     */
    public List<Integer> fetchAllCoopingShopIdsByShopAccountId(int shopAccountId) throws ShopAccountNotFoundException
    
    /**
     *  根据ShopId 查询所有关联的ShopAccount 包括关联的非阿波罗账号，有门店授权的账号，对应的管理员账户
     * @param shopId
     * @return
     */
    List<Integer> fetchAllShopAccountIdByShopId(int shopId);
    
    /**
     * 手艺人迁移使用、批量生成帐号（仅手艺人C端账号迁移使用）
     * @param createCraftsmanAccountList
     * @param ip
     * @return <shopId,account>的map
     * @throws AuthorizationCheckException
     */
    Map<Integer, Integer> batchCreateForSYR(List<CreateCraftsmanAccountDTO> createCraftsmanAccountList, String ip) throws AuthorizationCheckException;
    
    配置：
    
    <bean id="userAuthoriseService" class="com.dianping.dpsf.spring.ProxyBeanFactory" init-method="init">
        <property name="serviceName" value="http://service.dianping.com/merchantService/userAuthoriseService_1.0.0"/>
        <property name="iface" value="com.dianping.merchant.member.UserAuthoriseRPCService"/>
        <property name="serialize" value="hessian"/>
        <property name="callMethod" value="sync"/>
        <property name="timeout" value="5000"/>
    </bean>
    
    
ShopAccountService:

     /**
     * 更新帐号密码
     *
     * @param shopAccountId  商户账号ID
     * @param oldPassword    老密码
     * @param newPassword    新密码
     * @param ip             来源ip
     * @param modifyUserType 修改用户类型
     * @param modifyUserId   修改用户ID
     * @return
     */
    boolean updatePassword(int shopAccountId, String oldPassword, String newPassword, String ip, PasswordModifyUserTypeEnums modifyUserType, int modifyUserId);
    
    
    配置：
    
    <bean id="shopAccountService" class="com.dianping.dpsf.spring.ProxyBeanFactory" init-method="init">
        <property name="serviceName" value="http://service.dianping.com/merchantService/shopAccountService_1.0.0"/>
        <property name="iface" value="com.dianping.merchant.member.ShopAccountService"/>
        <property name="serialize" value="hessian"/>
        <property name="callMethod" value="sync"/>
        <property name="timeout" value="5000"/>
    </bean>
    
MtAccountService：

    public AccountFromMTDTO loadMTAccountById(int shopAccountId);
    
    <bean id="mtAccountService" class="com.dianping.dpsf.spring.ProxyBeanFactory" init-method="init">
        <property name="serviceName" value="http://service.dianping.com/merchantService/mtAccountService_1.0.0"/>
        <property name="iface" value="com.dianping.merchant.member.MtAccountService"/>
        <property name="serialize" value="hessian"/>
        <property name="callMethod" value="sync"/>
        <property name="timeout" value="5000"/>
    </bean>