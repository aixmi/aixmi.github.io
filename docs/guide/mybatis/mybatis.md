- <trim prefix="values (" suffix=")" suffixOverrides=",">
	1.<trim prefix="" suffix="" suffixOverrides="" prefixOverrides=""></trim>

prefix:在trim标签内sql语句加上前缀。

suffix:在trim标签内sql语句加上后缀。

suffixOverrides:指定去除多余的后缀内容，如：suffixOverrides=","，去除trim标签内sql语句多余的后缀","。

prefixOverrides:指定去除多余的前缀内容
```sql
 <insert id="insert" parameterType="com.xiaoju.framework.entity.CaseBackup">
    <selectKey resultType="java.lang.Long" order="AFTER" keyProperty="id">
      SELECT LAST_INSERT_ID() AS id
    </selectKey>
    insert into case_backup
    <trim prefix="(" suffix=")" suffixOverrides=",">
    <if test="id != null">
      id,
    </if>
      <if test="caseId != null">
        case_id,
      </if>
      <if test="title != null" >
        title,
      </if>
      <if test="creator != null">
        creator,
      </if>
      <if test="gmtCreated != null">
        gmt_created,
      </if>
      <if test="caseContent != null">
        case_content,
      </if>
      <if test="recordContent != null">
        record_content,
      </if>
      <if test="extra != null">
        extra,
    </if>
      <if test="isDelete != null">
      is_delete,
    </if>
    </trim>
   <trim prefix="values (" suffix=")" suffixOverrides=",">
    <if test="id != null">
      #{id,jdbcType=BIGINT},
    </if>
     <if test="caseId != null">
       #{caseId,jdbcType=BIGINT},
     </if>
     <if test="title != null">
       #{title,jdbcType=VARCHAR},
     </if>
     <if test="creator != null">
       #{creator,jdbcType=VARCHAR},
     </if>
       <if test="gmtCreated != null">
           #{gmtCreated,jdbcType=TIMESTAMP},
       </if>
     <if test="caseContent != null">
       #{caseContent,jdbcType=LONGVARCHAR},
     </if>
       <if test="recordContent != null">
           #{recordContent,jdbcType=LONGVARCHAR},
       </if>
       <if test="extra != null">
           #{extra,jdbcType=VARCHAR},
       </if>
       <if test="isDelete != null">
           #{isDelete,jdbcType=INTEGER},
       </if>
    </trim>
  </insert>
```


set 

```sql

 update exec_record
    <set>
      <if test="isDelete != null">
        is_delete = #{isDelete,jdbcType=INTEGER},
      </if>
      <if test="passCount != null">
        pass_count = #{passCount,jdbcType=INTEGER},
      </if>
      <if test="totalCount != null">
        total_count = #{totalCount,jdbcType=INTEGER},
      </if>
    </set>
    where id = #{id,jdbcType=BIGINT}
```
foreach
```sql

    select
    case_id as caseId, count(*) as recordNum
    from exec_record
    where case_id in
    <foreach collection="list" item="caseId" open="(" separator="," close=")">
      #{caseId}
    </foreach>
    and is_delete=0
    group by case_id
```
selectKey
```sql
  <insert id="insert" parameterType="com.xiaoju.framework.entity.TestCase">
    <selectKey resultType="java.lang.Long" order="AFTER" keyProperty="id">
      SELECT LAST_INSERT_ID() AS id
    </selectKey>
    insert into test_case
    <trim prefix="(" suffix=")" suffixOverrides=",">
      <if test="id != null">
        id,
      </if>
 
    </trim>
    <trim prefix="values (" suffix=")" suffixOverrides=",">
      <if test="id != null">
        #{id,jdbcType=BIGINT},
      </if>
   
    </trim>
  </insert>
```
```sql

<select id="getCountTestCase"  resultType="java.lang.Integer" >
    select count(*) from test_case
    <where>
      <if test="requirement_id == null">
        product_line_id = #{productLineId,jdbcType=BIGINT} and is_delete = 0 and case_type =#{case_type,jdbcType=INTEGER}
        <if test="id != null">
          and id = #{id,jdbcType=BIGINT}
        </if>
        <if test="creator != null and creator != ''">
          and creator = #{creator,jdbcType=VARCHAR}
        </if>
        <if test="beginTime != null">
          and gmt_created &gt;= #{beginTime,jdbcType=TIMESTAMP}
        </if>
        <if test="endTime != null">
          and gmt_created &lt;= #{endTime,jdbcType=TIMESTAMP}
        </if>
        <if test="title != null and title != ''">
          and title like CONCAT('%',#{title,jdbcType=VARCHAR},'%')
        </if>
        <if test="channel!= null ">
          and channel = #{channel,jdbcType=INTEGER}
        </if>
      </if>
      <if test="requirement_id != null">
        <foreach collection="requirement_id" item="item" separator="or">
          product_line_id = #{productLineId,jdbcType=BIGINT} and is_delete = 0 and case_type =#{case_type,jdbcType=INTEGER}
          <if test="id != null">
            and id = #{id,jdbcType=BIGINT}
          </if>
          <if test="creator != null and creator != ''">
            and creator = #{creator,jdbcType=VARCHAR}
          </if>
          <if test="requirement_id != null">
            and FIND_IN_SET(#{item,jdbcType=VARCHAR},requirement_id)
          </if>
          <if test="beginTime != null">
            and gmt_created &gt;= #{beginTime,jdbcType=TIMESTAMP}
          </if>
          <if test="endTime != null">
            and gmt_created &lt;= #{endTime,jdbcType=TIMESTAMP}
          </if>
          <if test="title != null and title != ''">
            and title like CONCAT('%',#{title,jdbcType=VARCHAR},'%')
          </if>
          <if test="channel!= null ">
            and channel = #{channel,jdbcType=INTEGER}
          </if>
        </foreach>
      </if>
    </where>
  </select>
```